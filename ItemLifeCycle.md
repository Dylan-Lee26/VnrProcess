# Tài liệu mô tả vòng đời công việc và vai trò của các vị trí

## 1. Mục tiêu

Tài liệu này mô tả chi tiết vòng đời của hai loại mục công việc chính: Bug và Task. Đồng thời phân tích vai trò và trách nhiệm của các vị trí tham gia chính: QC, SE, US và EM.

## 2. Khái niệm chung về vòng đời công việc

Mỗi mục công việc đi qua các trạng thái cơ bản sau:

- `Open`: Mục công việc mới được khởi tạo, đang chờ phân công.
- `Doing`: Mục công việc đang được thực hiện.
- `Resolve`: Mục công việc đã hoàn thành phần xử lý hoặc sửa lỗi, đang chờ kiểm thử / xác nhận.
- `Done`: Mục công việc đã chính thức hoàn tất và được triển khai.
  - Khi một task từ `Resolve` được đưa lên `Done`, nó phải đã được deploy lên môi trường kiểm thử/staging và đủ điều kiện để QC tiến hành test.
  - Với bug, sau khi deploy xong, QC có thể test lại để xác nhận lỗi đã được khắc phục triệt để.

Trên sơ đồ cũng tồn tại các luồng phản hồi chính:

- `Test Fail`: Khi kiểm thử không đạt, mục công việc quay lại trạng thái `Doing` để sửa lại.
- `Change`: Khi có yêu cầu thay đổi hoặc điều chỉnh, mục công việc quay lại `Open` hoặc `Doing`.

## 3. Vai trò các vị trí

### 3.1 QC (Quality Control)

Vai trò chính:

- Phát hiện và ghi nhận lỗi (bug).
- Tạo và mở các issue bug trong hệ thống.
- Kiểm thử lại sau khi SE đã sửa lỗi.
- Xác nhận lỗi đã được khắc phục trước khi chuyển mục công việc về `Done`.

Luồng công việc của QC:

1. QC phát hiện lỗi -> tạo `Bug` và chuyển sang trạng thái `Open`.
2. Bug được chuyển sang `Doing` khi SE bắt đầu sửa.
3. Sau khi SE hoàn thành sửa, bug vào trạng thái `Resolve`.
4. QC kiểm thử lại, nếu `Test Fail` thì bug quay về `Doing`.
5. Nếu test đạt, QC xác nhận và bug vào trạng thái `Done`.

### 3.2 SE (Software Engineer)

Vai trò chính:

- Nhận task hoặc bug từ hệ thống và bắt đầu triển khai.
- Phân tích yêu cầu, thiết kế giải pháp, viết code và kiểm thử cơ bản.
- Triển khai fix bug hoặc hoàn thành feature.
- Xử lý các yêu cầu thay đổi (change request) nếu có.

Luồng công việc của SE:

1. SE nhận `Task` hoặc `Bug` ở trạng thái `Open`.
2. SE chuyển item sang `Doing` và thực hiện công việc.
3. Khi hoàn thành phần code/fix, SE đưa item vào `Resolve`.
4. Nếu cần hiệu chỉnh do `Test Fail` hoặc `Change`, SE tiếp tục xử lý và quay lại `Doing`.
5. SE phối hợp với QC để đưa item đến `Done`.

### 3.3 US (User Story / Yêu cầu nghiệp vụ)

Vai trò chính:

- Đại diện cho dòng công việc liên quan đến yêu cầu nghiệp vụ.
- Theo dõi tiến trình `Open` -> `Doing` -> `Done`.
- Xác định rõ yêu cầu ban đầu, kiểm tra sau khi hoàn thành.

Luồng công việc của US:

1. Yêu cầu nghiệp vụ được tạo và mở ở trạng thái `Open`.
2. Khi đội phát triển bắt đầu làm, item chuyển sang `Doing`.
3. Sau khi hoàn thành và triển khai, item vào trạng thái `Done`.
4. Nếu có thay đổi, item có thể trở lại `Open` hoặc `Doing` để cập nhật.

### 3.4 EM (Engineering Manager)

Vai trò chính:

- Xác nhận cuối cùng cho các mục công việc khi hoàn tất.
- Quản lý và điều phối nhóm trong toàn bộ quy trình.
- Đảm bảo chất lượng, tính khả thi và tuân thủ quy trình.

Luồng công việc của EM:

1. EM xem xét và xác nhận khi một mục công việc đã sẵn sàng kết thúc.
2. Nếu cần điều chỉnh hoặc bổ sung, EM có thể điều phối yêu cầu quay lại giai đoạn `Open` hoặc `Doing`.
3. EM chịu trách nhiệm cấp phát nguồn lực và xác thực sự hoàn chỉnh của công việc.

## 4. Phân biệt Bug và Task

### 4.1 Bug

- Được QC phát hiện hoặc báo cáo.
- Quy trình: `Open` -> `Doing` -> `Resolve` -> `Done`.
- Có luồng `Test Fail` rõ ràng, giúp đảm bảo lỗi được sửa triệt để.
- Có thể được chuyển lại `Change` nếu yêu cầu chỉnh sửa phát sinh.

### 4.2 Task

- Được SE hoặc đội phát triển tạo ra để thực hiện feature, cải tiến hoặc công việc kỹ thuật.
- Quy trình: `Open` -> `Doing` -> `Resolve` -> `Done`.
- Nếu kiểm thử không đạt hoặc cần bổ sung, task quay lại `Doing`.
- Có thể cập nhật bởi `Change` nếu yêu cầu mới phát sinh.

## 5. Luồng tương tác giữa các vị trí

- QC khởi tạo và kiểm thử bug.
- SE thực hiện sửa lỗi và hoàn thiện task.
- EM theo dõi tiến độ yêu cầu nghiệp vụ và đảm bảo công việc đúng với mong đợi xác nhận hoàn tất và điều phối nếu có thay đổi.

## 6. Kết luận

Chu trình này đảm bảo rằng:

- Các bug được phát hiện, sửa và kiểm thử một cách rõ ràng.
- Task được thực hiện theo các bước minh bạch từ mở đến hoàn thành.
- Mọi thay đổi đều có luồng phản hồi (`Test Fail`, `Change`) để bảo đảm chất lượng.
- Vai trò QC, SE, US và EM được phân định rõ, giúp chủ động theo dõi trách nhiệm.

Với sơ đồ mô tả vòng đời này, đội phát triển có thể vận hành quy trình hiệu quả, giảm thiểu sai sót và tối ưu hóa sự phối hợp giữa các chức năng.