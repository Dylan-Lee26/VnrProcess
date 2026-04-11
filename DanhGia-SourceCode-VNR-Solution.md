# Đánh giá source code — VNR.Solution

**Phạm vi:** Toàn bộ solution tại nhánh làm việc hiện tại (snapshot đánh giá).  
**Ngày tham chiếu:** 2026-04-09  
**Phương pháp:** Rà soát cấu trúc solution, quy ước `.cursor/rules`, mẫu code lõi (`VNR.Core.Api`, `VNR.Core.Application`), thống kê file, và cấu hình mẫu — không chạy full static analysis toàn solution.

---

## 1. Tóm tắt điều hành

| Tiêu chí | Nhận xét ngắn | Điểm mạnh / rủi ro |
|----------|----------------|---------------------|
| Kiến trúc | Microservice theo bounded context (HRE, Evaluation, Training, System, Shared, Succession, Notification, Logging, Sample, IdentityServer, Gateway, Worker). Lõi dùng Clean Architecture + CQRS. | Mạnh: tách service rõ, tái sử dụng `VNR.Core.*`. |
| Quy mô | ~82 project `.csproj`, ~6500+ file `.cs` dưới `Src`, hàng trăm controller, ~800+ file `*Handler.cs`. | Mạnh: năng lực xử lý nghiệp vụ lớn. Rủi ro: cần governance chặt để không phình pattern. |
| Công nghệ | .NET 8 (đa số), MediatR 12.5, FluentValidation, EF/Dapper (theo infrastructure), Serilog, Ocelot (gateway), IdentityServer. | Mạnh: stack hiện đại, đồng nhất MediatR. |
| Chất lượng & test | Test project tồn tại nhưng chủ yếu placeholder (`UnitTest1` trống). | **Rủi ro cao:** thiếu bảo hiểm hồi quy tự động. |
| Bảo mật cấu hình | Secret/connection string xuất hiện trong file JSON trong repo (đã phát hiện qua quét mẫu). | **Rủi ro cao:** cần tách secrets khỏi Git. |
| SQL động / store | Dynamic grid gọi SP theo `StoreName`; PostgreSQL build câu lệnh bằng nối chuỗi tham số. | **Rủi ro:** cần whitelist SP + sửa tham số hóa đúng chuẩn trên PG (xem mục 6.1). |
| Vận hành | Có HealthChecks UI, logging tập trung, worker host, dockerfile rải rác. | Ổn; cần chiến lược deploy/container thống nhất (mục 8.1). |

**Kết luận ngắn:** Codebase **mature về kiến trúc và quy ước nội bộ**, phù hợp domain HR phức tạp; điểm yếu lớn nhất là **kiểm thử tự động mỏng** và **quản lý bí mật trong cấu hình**.

---

## 2. Kiến trúc & tổ chức

### 2.1 Phân lớp chuẩn

- **Cores:** `VNR.Core`, `VNR.Core.Api`, `VNR.Core.Application`, `VNR.Core.Domain`, `VNR.Core.Models`, `VNR.Core.Common`, `VNR.Core.Configurations`, `VNR.Core.Security` — nền tảng dùng chọn.
- **Infrastructure:** Persistence, BaseRepositories, Security, Jobs, Queue, Notification, Permission, Translation, Logging, EntityFramework (SQL Server/PostgreSQL), v.v.
- **Services:** Mỗi service thường có `Api` / `Application` / `Domain` / `Infrastructure` / `Models` (tùy service).
- **Hosting:** `CompositionRoot`, `WorkerHost` — điểm tập trung DI/hosting.
- **Tools & Monitoring:** công cụ migrate, security, password generator (untracked trên một số clone), HealthChecks UI.

Phù hợp với quy tắc trong `.cursor/rules` (clean-architecture, application-rule, infrastructure-rule, domain-rule).

### 2.2 Phụ thuộc giữa các tầng

- Controller kế thừa `BaseApiController`, gọi `IMediator` — **đúng hướng controller mỏng**.
- Application chứa command/query/handler — **CQRS rõ ràng** (số lượng handler rất lớn).

### 2.3 Điểm cần lưu ý

- **Đồng nhất .NET:** Đa số `net8.0`; có project test khai báo `net9.0` — nên thống nhất để tránh ma trận SDK/runtime khi CI.
- **Không thấy `Directory.Build.props` ở root** — có thể thiếu chỗ tập trung Version, Nullable, AnalysisLevel (tùy chính sách team).

---

## 3. Stack công nghệ & thư viện

| Lĩnh vực | Ghi nhận |
|----------|----------|
| Runtime | .NET 8 (chủ đạo) |
| API | ASP.NET Core, `ApiController`, MediatR |
| Validation | FluentValidation + pipeline `ValidationBehavior` |
| Mapping / JSON | AutoMapper (theo quy ước dự án), Newtonsoft.Json trong `BaseApiController` |
| Auth | IdentityServer, JWT/Secret trong config (cần review vận hành) |
| Data | EF + Dapper (theo module), stored procedure + Grid config (theo `.cursorrules`) |
| Log | Serilog (config `Config/Serilog.json`), `ILoggingService` trong controller base |
| Gateway | Ocelot (`VNR.Service.OcelotGateway`) |
| Background | Hangfire (connection trong config), Workers |

**Đánh giá:** Stack **phù hợp enterprise .NET**; có **hai stack JSON** (Newtonsoft ở base API vs STJ mặc định của ASP.NET) — cần conscious choice để tránh double serialization hoặc khác biệt contract.

---

## 4. Chất lượng code & tuân thủ pattern

### 4.1 Điểm mạnh

- **CQRS + MediatR** được áp dụng rộng (hàng trăm handler).
- **`BaseApiController.HandleRequest`** gom xử lý exception: `BusinessException`, validation, `FileNotFoundException`, generic — có log id, localize message 500, che chi tiết lỗi ngoài DEBUG.
- **Pipeline validation** MediatR (`ValidationBehavior`) — chuẩn cross-cutting.
- **Quy ước Cursor** chi tiết (controller, api-controller, cqrs-mediatr, query-list-grid, repository-uow, SP rules) — giảm drift khi nhiều dev.

### 4.2 Rủi ro / nợ kỹ thuật

- **Kích thước `BaseApiController`:** Nhiều nhánh `catch`, mix concern (serialization settings, file result, localization) — có thể tách filter/middleware hoặc exception handler toàn cục để dễ test.
- **So khớp tên/type bằng string** cho `FileStreamResult` — dễ vỡ khi refactor; nên dùng pattern trả về rõ ràng (ví dụ wrapper hoặc `IActionResult` từ handler).
- **Typo nhỏ trong log/message** (ví dụ `mesageError`) — không ảnh hưởng chức năng nhưng nên sửa dần trong hygiene pass.
- **Độ phức tạp cyclomatic** có thể cao ở một số handler lớn — nên đo bằng analyzer nếu team có chuẩn.

---

## 5. API & hợp đồng (contract)

- Response thống nhất qua `IApiResult` / `ApiResult` / grid Kendo — **phù hợp frontend Kendo**.
- Grid: `BaseRequestGridModel`, `BaseResponseGridModel<T>` — nhất quán với rule nội bộ.
- **Khuyến nghị:** OpenAPI/Swagger mô tả đầy đủ + versioning nếu public nhiều client; kiểm tra breaking change khi đổi DTO.

---

## 6. Bảo mật

| Chủ đề | Đánh giá |
|--------|----------|
| Secrets trong repo | **Cần hành động:** chuyển sang User Secrets / Azure Key Vault / biến môi trường; rotate secret đã lộ trong lịch sử Git. |
| JWT / Identity | Có layer IdentityServer — cần đảm bảo clock skew, refresh, revoke token theo chính sách. |
| SQL (tổng quan) | EF Core và phần lớn Dapper dùng tham số hóa — hướng đúng. |
| SQL (cần hạn chế) | Còn **nối chuỗi** tên bảng/cột/`ORDER BY`/điều kiện động ở base repository và vài service — **bắt buộc** không lấy từ input client hoặc phải allowlist (chi tiết mục **6.2**). |
| API đặc quyền | Có module Permission — cần audit định kỳ endpoint nhạy cảm (BFF Shared, migration script, dynamic controller). |

### 6.1 SQL injection & truy vấn động

**Luồng an toàn hơn**

- Truy vấn qua **EF Core** và **Dapper** với object/`DynamicParameters`: tham số tách khỏi text SQL — giảm injection cổ điển.
- `DynamicService.GetDataSourceComboboxEntity`: có **`SafeSqlIdentifier`**, kiểm tra entity khớp metadata, **`ContainsSqlInjectionRisk`** cho `Sort`/`Query` — mẫu phòng thủ tốt cho SQL động có giới hạn.

**Luồng cần kiểm soát chặt**

- API **`GetDataSourceByDynamicStore`** / **`GetDataSourceByStandardStore`** (`DynamicController`, service `DynamicService`): client gửi **`GridName` / `StoreName`**; server resolve cấu hình qua `IConfigSQLStoreService`, build cột/order qua `SqlStoreHelper` + `QuoteClause` — **cột/sort được ràng buộc theo config** là điểm tích cực.
- **`StoreName` đi thẳng tới tên SP** trong `QueryDynamicStoredProcedureAsync` (`DapperQueryService`). Trên **SQL Server**, Dapper dùng `CommandType.StoredProcedure` — tên procedure không nằm trong chuỗi SQL tự do; tuy vậy **bắt buộc** chỉ cho phép các SP đã đăng ký (whitelist theo `GridConfigStore` / danh sách cố định), không tin tưởng chuỗi từ client.
- Trên **PostgreSQL**, `PostgreSqlDialect.GetDynamicStoredProcedure` hiện **ghép tham số bằng nối chuỗi** (`key := 'value'`) và function name trong câu `SELECT * FROM "procName"(...)`. Đây là **mô hình dễ bị SQL injection** nếu giá trị (ví dụ nội dung JSON filter, chuỗi người dùng) chứa dấu nháy hoặc ký tự đặc biệt; đồng thời tên hàm/SP cần **allowlist**, không lấy trực tiếp từ request không kiểm chứng.

**Khuyến nghị kỹ thuật (SQL)**

1. Với PostgreSQL: **bỏ nối giá trị vào SQL text**; dùng placeholder tham số tương thích Npgsql/Dapper (hoặc chỉ gọi procedure qua API driver với tham số riêng), đồng nhất với cách SQL Server đang làm.
2. **Allowlist** `StoreName`/`procName` sau khi đối chiếu với config đã load (và từ chối mọi tên không có trong map).
3. Rà soát **stored procedure** nhận `sp_filter_json` / JSON filter: parse an toàn, không `EXEC` động từ nội dung JSON.
4. Thêm **test tự động** (security): payload điển hình SQLi cho các endpoint dynamic/combobox.

### 6.2 Các mục SQL injection cần được hạn chế / kiểm soát

Mục này liệt kê **vị trí và kiểu rủi ro** để team **ưu tiên rà soát**, **siết allowlist**, hoặc **refactor** (không thay thế audit pentest).

#### Nguyên tắc chung (nên áp dụng toàn solution)

- **Không** đưa chuỗi từ HTTP request trực tiếp vào: tên bảng/view/SP/cột, mệnh đề `ORDER BY`, mảnh `WHERE` đã ghép sẵn.
- Ưu tiên: **tham số hóa** (Dapper `@{name}`, EF `ExecuteSqlRaw` + placeholder `{0}` kèm mảng tham số), hoặc **map sang hằng** (enum/whitelist trong code).
- Mọi chỗ **còn bắt buộc nối identifier** (tên bảng) phải qua **một lớp duy nhất** (ví dụ `SafeSqlIdentifier` + danh sách bảng cho phép), có **unit test** cho ký tự tấn công (`'`, `"`, `;`, `--`, `/*`).

#### Bảng: khu vực cần hạn chế / giảm rủi ro

| STT | Khu vực / mẫu | Rủi ro chính | Hạn chế & xử lý đề xuất |
|-----|----------------|--------------|-------------------------|
| 1 | `PostgreSqlDialect.GetDynamicStoredProcedure` | Ghép `procName` và **toàn bộ giá trị tham số** vào một chuỗi SQL (`'...'`). | **Ưu tiên cao:** bỏ literal trong SQL; dùng tham số Npgsql/Dapper; allowlist tên function/SP. |
| 2 | `DynamicService` + API `GetDataSourceBy*Store` | `StoreName` quyết định SP; filter JSON đổ xuống SP. | Allowlist store/SP; SP không được `EXEC`/`format` SQL từ nội dung JSON. |
| 3 | `DapperRepositoryBase` (`GetPagedAsync`, …) | `ORDER BY {orderClause}` và `OFFSET`/`FETCH` nằm trong chuỗi; **`orderBy` từ ngoài** = vector ORDER BY injection. | Chỉ cho phép cột trong whitelist; hoặc map `sortKey` → cột cố định; không truyền thẳng chuỗi client. |
| 4 | `DapperRepositoryBase` (CRUD generic) | `tableName` được nối vào `SELECT/INSERT/UPDATE/DELETE`. | `tableName` **chỉ** từ hằng/map nội bộ (typeof → tên bảng đã kiểm); không từ request. |
| 5 | `DapperRepositoryBase.SearchAsync` | Điều kiện `WHERE` build từ **tên property** của object tìm kiếm. | Dùng DTO cố định; không dùng `dynamic`/dictionary từ client làm `searchCriteria`. |
| 6 | `HreProfileService`, `HrmTestDapperService` (mẫu) | SQL dạng `$"SELECT ... {TableName} ... {whereClause}"`. | `TableName`/`whereClause` phải nội bộ đã validate; lý tưởng chuyển sang repository có tham số. |
| 7 | `CatOrgStructureService`, `CatPositionService` (PG) | Nối `'{parent.Id}'` vào SQL (Guid thường an toàn hơn chuỗi tự do nhưng vẫn là **anti-pattern**). | Dùng tham số `@p` / `split_guids` với argument parameterized. |
| 8 | `SqlServerDialect` / `PostgreSqlDialect` `BuildSelectComboboxSql` | Identifier đã qua `SafeSqlIdentifier`; phần `query`/`sort` cần parse chặt. | Giữ rule parse hiện tại; bổ sung test cho chuỗi query độc. |
| 9 | `HttpIntergrationService` gọi `QueryDynamicStoredProcedureAsync` với `config.Endpoint` | Nếu endpoint cấu hình trỏ tới tên SP/function do người cấu hình nhập sai/không kiểm — mở rộng bề mặt. | Validate `Endpoint` theo allowlist khi load config. |
|10 | `ExecuteSqlRaw` / `FromSqlRaw` rải rác | An toàn **nếu** SQL là template cố định và tham số tách riêng; nguy hiểm nếu nối biến vào chuỗi. | Code review rule: mọi `*SqlRaw*` phải có checklist (template + parameters). |
|11 | Log SQL (ví dụ `DapperQueryService` log full `sql`) | Không phải injection nhưng **lộ dữ liệu** qua log. | Dùng `SqlSanitizer` / tắt log tham số trên production. |

#### Hành động tổ chức (ngoài code)

- **SAST:** bật rule cảnh báo string interpolation + SQL (Roslyn/Sonar).
- **Quy trình PR:** checklist “có chuỗi SQL động không? input từ đâu?”.
- **Regression:** thêm vài case tích hợp gửi payload SQLi vào grid dynamic + combobox + endpoint có sort.

---

## 7. Dữ liệu & persistence

- **Đa database:** SQL Server và PostgreSQL (project EF PostgreSQL) — phù hợp từng service; cần tài liệu hóa “service nào dùng DB nào”.
- **Repository + UoW** (theo rule) — giảm logic SQL trong controller.
- **Migrate:** tool `VNR.Tool.UpdateMigrateDb` — tốt cho vận hành; cần quy trình migration trên môi trường.

---

## 8. Tích hợp & hạ tầng chạy

- **Gateway (Ocelot):** tập trung route — cần đồng bộ với service discovery/health nếu scale.
- **Notification / FCM / Mail:** service riêng — tách biên tốt.
- **Worker / Hangfire:** phù hợp job nền; theo dõi failed job và idempotency.

### 8.1 Deploy & Docker hóa

**Hiện trạng trong repo**

- **`.dockerignore`** (root) loại trừ `**/Dockerfile*`, `**/docker-compose*`, `**/bin`, `**/obj`, v.v. — giúp context build gọn; lưu ý: nếu pipeline cần **copy Dockerfile làm ngữ cảnh** thì phải điều chỉnh pattern (hiện `!` không carve-out Dockerfile).
- **`VNR.Service.Evaluation.Api/dockerfile`:** multi-stage `sdk:8.0` → `aspnet:8.0`, publish project API, copy thư mục `Config`, `EXPOSE`, `ENTRYPOINT` — mẫu **đủ dùng cho một service**; `ASPNETCORE_ENVIRONMENT=Development` trong image **không nên** dùng cho production (chuyển sang `Production` + cấu hình qua env/secrets).
- **`VNR.Service.IdentityServer.Identity/Dockerfile`:** .NET **6.0**, copy từng `.csproj` để tối ưu layer cache — **lệch major** so với phần lớn solution **.NET 8**; cần roadmap nâng cấp hoặc ghi rõ lý do giữ 6.0.
- **`VNR.Tool.PasswordGenerator/Dockerfile`:** theo template VS “customize container”.
- **Đa số API khác** (HRE, System, Shared, Training, …) **chưa thấy Dockerfile** trong solution — deploy có thể đang dẫn xuất IIS/Windows Service/Azure App Service ngoài container.

**Hướng chuẩn hóa deploy container**

| Hạng mục | Gợi ý |
|----------|--------|
| Image | Một Dockerfile / service (hoặc matrix build CI), base `mcr.microsoft.com/dotnet/aspnet:8.0`, non-root user khi khả thi. |
| Cấu hình | **Không** bake `Config/*.json` chứa secret; mount volume hoặc env + Key Vault; `ASPNETCORE_ENVIRONMENT` theo môi trường. |
| Build | `docker build` từ root repo với `-f` trỏ từng service; hoặc `docker compose` với profile theo nhóm service. |
| Gateway | Container Ocelot phía trước; service nội bộ không public trực tiếp. |
| Health | `HEALTHCHECK` trong image hoặc probe Kubernetes/App Service trỏ endpoint health của từng API. |
| CI/CD | Pipeline build → scan image (optional) → push registry → deploy với tag version; đồng bộ với ma trận test (mục 10). |

---

## 8.2 Hướng loại bỏ / thay thế API “store” động (`GetDataSourceBy*Store`)

Trong codebase, các endpoint tương đương “get theo store” chủ yếu là:

- `POST .../Dynamic/GetDataSourceByDynamicStore`
- `POST .../Dynamic/GetDataSourceByStandardStore`

(cùng họ với các API combobox/filter dynamic khác trên `DynamicController`).

**Vì sao nên giảm dần**

- **Bề mặt tấn công rộng:** tên store/SP và filter từ client → khó audit và khó kiểm thử từng luồng.
- **Hợp đồng API kém rõ:** OpenAPI/consumer khó biết schema từng màn hình.
- **Phụ thuộc JSON grid config** (`GridConfigStore`, …) — hợp lý cho tốc độ triển khai nhưng **nợ kỹ thuật** khi cần type-safety và governance.

**Lộ trình đề xuất (incremental)**

1. **Đóng băng danh mục store:** Export danh sách `(GridName, StoreName)` đang dùng từ frontend + config; đánh dấu cái nào **active** / deprecated.
2. **Thay từng nhóm màn hình** bằng **Query/Handler MediatR** cụ thể (ví dụ `GetXxxGridQuery`, `GetXxxDropdownQuery`) trả DTO rõ ràng; controller mỏng gọi `HandleRequest` như pattern hiện tại.
3. **Adapter tạm:** Handler mới **nội bộ gọi lại** `IDynamicService` / config cũ cho đến khi migrate xong SP hoặc chuyển sang repository — giảm big-bang.
4. **Chính sách allowlist:** Trong thời gian song song, **chặn mọi `StoreName` không nằm trong whitelist** (sync từ bước 1).
5. **Phiên bản API:** Đánh dấu `[Obsolete]` + header deprecation cho endpoint cũ; frontend/feature flag chuyển dần sang route mới.
6. **Xóa hẳn** khi không còn consumer và đã có test regression cho từng màn hình thay thế.

**Tiêu chí “xong” một màn hình:** không còn gọi `GetDataSourceByDynamicStore`/`StandardStore` cho grid đó; có integration test hoặc contract test cho query mới.

---

## 9. Quan sát được (observability)

- Serilog + file cấu hình.
- Logging qua `ILoggingService` gắn `TraceIdentifier` — hỗ trợ truy vết.
- HealthChecks UI — nên gắn vào dashboard và alert.

---

## 10. Kiểm thử & CI/CD

- **Unit/Integration test:** hầu như chưa có nội dung thực (placeholder).
- **Không thấy pipeline `.github/workflows`** trong workspace — có thể CI nằm ngoài repo hoặc chưa bật.

**Khuyến nghị:**  
1) Thêm test cho `VNR.Core.Application` (validators, behaviors, pure domain).  
2) Contract test hoặc integration test cho vài luồng critical (auth, payroll-like HRE).  
3) SonarQube / Roslyn analyzers + nullable reference types theo từng module.

---

## 11. Tài liệu & quy trình AI/human

- `.cursorrules` + `.cursor/rules/*.mdc` mô tả rõ pattern — **tài sản tốt** cho onboarding và AI-assisted coding.
- Thư mục `memory-bank` được rule nhắc tới nhưng **có thể chưa được khởi tạo đầy đủ** trên clone — nên bổ sung nếu team dùng Memory Bank.

---

## 12. Ma trận ưu tiên cải thiện

| Ưu tiên | Hạng mục | Hành động gợi ý |
|---------|----------|-----------------|
| P0 | Secrets | Loại bỏ khỏi Git, rotate, template `appsettings.Development.json` + env |
| P0 | SQL PG dynamic store | Sửa `GetDynamicStoredProcedure` (PostgreSQL): không nối giá trị vào SQL; allowlist `StoreName` |
| P0 | Test | Seed test thật cho core behaviors + 1–2 integration tests |
| P1 | Exception handling | Global exception handler / ProblemDetails, giảm logic trong base controller |
| P1 | Đồng nhất TF | `net8.0` toàn solution hoặc lý do rõ cho `net9.0` |
| P1 | API store động | Lộ trình thay `GetDataSourceBy*Store` bằng query có kiểu + whitelist tạm |
| P2 | Build chung | `Directory.Build.props` (nullable, warnings as errors từng phase) |
| P2 | Container | Dockerfile theo service .NET 8, healthcheck, env production, compose/gateway |

---

## 13. Kết luận

**VNR.Solution** là codebase **lớn, có kiến trúc phân service và CQRS nhất quán với quy ước nội bộ**, phù hợp hệ thống HR đa module. Điểm nổi bật là **tái sử dụng core chung** và **số lượng handler/command/query phản ánh nghiệp vụ sâu**. Hướng cải thiện quan trọng nhất là **an toàn cấu hình**, **kiểm thử tự động**, **an toàn truy vấn động (đặc biệt PostgreSQL + store SP)**, **chuẩn hóa container/deploy**, **giảm dần API grid theo store** thông qua query có kiểu, và **tinh gọn cross-cutting (exception/JSON)** khi tiếp tục mở rộng.

---

*Tài liệu này là đánh giá tĩnh; để có điểm số định lượng (coverage, duplication, complexity) nên bổ sung chạy `dotnet test`, `dotnet format`, và công cụ phân tích tĩnh trên build server.*
