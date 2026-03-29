# TÀI LIỆU QUY TRÌNH KIỂM THỬ PHẦN MỀM

## 1. Mục tiêu chung

- Đảm bảo hệ thống hoạt động đúng theo yêu cầu nghiệp vụ.
- Phát hiện sớm lỗi, giảm thiểu rủi ro khi triển khai.
- Cải thiện chất lượng trải nghiệm người dùng.
- Đảm bảo sự ổn định và hiệu suất đủ tốt trong môi trường thực tế.

## 2. Tổng quan quy trình kiểm thử

Quy trình kiểm thử được thực hiện theo thứ tự:

1. Test API (kiểm thử chức năng lõi, backend).
2. Test UX/UI (kiểm thử giao diện, trải nghiệm người dùng).
3. Test End-to-End (kiểm thử luồng nghiệp vụ toàn diện).

**Nguyên tắc chính:**

- Test từ core logic → giao diện → luồng tổng thể.
- Fail sớm càng tốt (shift-left testing).
- Kết hợp giữa kiểm thử thủ công và tự động để tối ưu.

---

## 3. Quy trình thực hiện chung

### 3.1 Chuẩn bị

- Thu thập tài liệu yêu cầu, API spec, thiết kế UI, luồng nghiệp vụ.
- Xác định phạm vi kiểm thử: chức năng nào cần test, độ ưu tiên.
- Chuẩn bị dữ liệu test và môi trường test phù hợp.

### 3.2 Thiết kế test case

- Viết test case chi tiết theo từng chức năng.
- Mỗi test case gồm: ID, tiêu đề, mô tả, bước thực hiện, dữ liệu đầu vào, kỳ vọng.
- Phân loại test case: happy path, negative, boundary, regression.

### 3.3 Thực hiện kiểm thử

- Thực hiện từng test case theo thứ tự ưu tiên.
- Ghi nhận kết quả và bằng chứng (screenshot, response body, log).
- Đánh giá kết quả: Pass / Fail / Blocked.

### 3.4 Báo cáo và đóng vòng

- Tổng hợp kết quả kiểm thử.
- Báo cáo lỗi, mức độ nghiêm trọng và trạng thái.
- Kiểm tra lại các test case đã fail sau khi sửa lỗi.

---

## 4. TEST API

### 4.1 Mục tiêu

- Xác nhận endpoint hoạt động đúng phương thức và dữ liệu.
- Kiểm tra đầu vào/đầu ra theo spec.
- Validate business rule, xử lý lỗi và trạng thái HTTP.
- Đảm bảo tính bảo mật, xác thực và phân quyền.

### 4.2 Phạm vi test API

| Hạng mục | Nội dung |
| --- | --- |
| Endpoint | URL, HTTP method, version API |
| Request | Headers, path params, query params, body |
| Response | Status code, schema, dữ liệu trả về |
| Business Logic | Điều kiện nghiệp vụ, phép tính, trạng thái |
| Validation | Required field, datatype, format, limit |
| Error Handling | Thông báo lỗi, mã lỗi, fallback |
| Performance | Response time, tải cơ bản |

### 4.3 Các bước thực hiện

1. Xem tài liệu API (OpenAPI/Swagger, Postman Collection).
2. Lập danh sách endpoint cần test.
3. Viết test case cho các trường hợp: success, invalid input, unauthorized, not found.
4. Thực hiện bằng Postman, hoặc tool tự động.
5. Ghi nhận kết quả và so sánh với expected.
6. Kiểm tra response schema và mã trạng thái.

### 4.4 Checklist API

- [ ] URL và method đúng.
- [ ] Status code chính xác (200/201/400/401/403/404/500...).
- [ ] Response body đúng schema.
- [ ] Giá trị dữ liệu đúng và đầy đủ.
- [ ] Validation input hoạt động.
- [ ] Các rule nghiệp vụ được áp dụng.
- [ ] Xử lý lỗi rõ ràng.
- [ ] Authentication/authorization đúng.
- [ ] Response time chấp nhận được.

---

## 5. TEST UX/UI

### 5.1 Mục tiêu

- Đảm bảo giao diện tuân thủ thiết kế.
- Đảm bảo trải nghiệm người dùng mượt mà.
- Kiểm tra hiển thị, tương tác và độ phản hồi.

### 5.2 Phạm vi test UX/UI

| Hạng mục | Nội dung |
| --- | --- |
| Layout | Bố cục, lưới, khoảng cách |
| Visual | Màu sắc, font, icon, hình ảnh |
| Tương tác | Click, hover, form submit |
| Validation | Thông báo lỗi, hint, required |
| Responsive | Desktop, tablet, mobile |
| Accessibility | Tab order, keyboard, screen reader |
| Cross-browser | Chrome, Edge, Firefox, Safari |

### 5.3 Các bước thực hiện

1. Đọc yêu cầu thiết kế và chuẩn UI.
2. Chuẩn bị checklist dựa trên design và user flow.
3. Thực hiện kiểm tra từng màn hình.
4. Chụp ảnh màn hình và lưu bằng chứng.
5. Ghi lại các bất thường, lỗi hiển thị, lỗi tương tác.
6. Review với team thiết kế nếu cần.

### 5.4 Checklist UX/UI

- [ ] Giao diện đúng với mockup/Figma.
- [ ] Không bị vỡ layout.
- [ ] Các button/element hoạt động.
- [ ] Form validation đúng.
- [ ] Responsive hiển thị tốt.
- [ ] Thông báo lỗi rõ ràng.
- [ ] Không có nội dung tràn.
- [ ] Trải nghiệm người dùng hợp lý.

---

## 6. TEST END-TO-END (E2E)

### 6.1 Mục tiêu

- Kiểm tra flow nghiệp vụ từ đầu đến cuối.
- Xác nhận các thành phần tích hợp với nhau đúng.
- Đảm bảo dữ liệu luân chuyển chính xác qua các bước.

### 6.2 Phạm vi test E2E

| Hạng mục | Nội dung |
| --- | --- |
| Business Flow | Luồng chính, luồng phụ |
| Integration | API ↔ UI, service ↔ service |
| Data Flow | Lưu trữ, truy xuất, cập nhật |
| Error Recovery | Thu hồi khi có lỗi |
| Regression | Kiểm tra sau sửa lỗi |

### 6.3 Các bước thực hiện

1. Xác định luồng chính và các luồng phụ.
2. Viết test case E2E theo kịch bản thực tế.
3. Thực hiện bằng công cụ automation hoặc thủ công.
4. Ghi lại các bước thành công/không thành công.
5. Kiểm tra ảnh hưởng của lỗi ở bước tiếp theo.
6. Chạy lại sau khi fix lỗi để đảm bảo không hồi quy.

### 6.4 Checklist E2E

- [ ] Luồng nghiệp vụ hoàn chỉnh.
- [ ] Dữ liệu đi qua các bước đúng.
- [ ] Không có lỗi integration.
- [ ] Người dùng không bị chặn giữa chừng.
- [ ] Tính toán/trạng thái chính xác.
- [ ] Regresion test với các chức năng liên quan.

---

## 7. Chiến lược kiểm thử

### 7.1 Test Pyramid

- API tests: nhiều nhất.
- UX/UI tests: vừa phải.
- E2E tests: ít nhưng quan trọng.

### 7.2 Agile Mapping

- Dev: Unit test, API test.
- QA: API → UI → E2E.
- Release: Regression test, Smoke test.

### 7.3 Định nghĩa DONE

- [ ] API pass.
- [ ] UI pass.
- [ ] E2E pass.
- [ ] Không có bug nghiêm trọng.
- [ ] Tài liệu test hoàn chỉnh.
- [ ] Báo cáo kết thúc rõ ràng.

---

## 8. Mẫu test case

### 8.1 Mẫu test case API

| ID | Tiêu đề | Mô tả | Bước thực hiện | Dữ liệu test | Kỳ vọng | Kết quả | Ghi chú |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-API-001 | Đăng nhập thành công | Kiểm tra API đăng nhập với email hợp lệ | 1. Gọi POST /api/auth/login 2. Gửi body chứa email và password đúng | {"email":"user@test.com","password":"Pass123!"} | Trả về 200, token và thông tin user | Pass / Fail | Response có token |
| TC-API-002 | Đăng nhập sai mật khẩu | Kiểm tra trả về lỗi khi password sai | 1. Gọi POST /api/auth/login 2. Gửi password sai | {"email":"user@test.com","password":"Wrong123"} | Trả về 401, message "Sai mật khẩu" | Pass / Fail | |

### 8.2 Mẫu test case UX/UI

| ID | Tiêu đề | Mô tả | Bước thực hiện | Dữ liệu test | Kỳ vọng | Kết quả | Ghi chú |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-UI-001 | Màn hình đăng nhập | Kiểm tra hiển thị form đăng nhập | 1. Mở trang đăng nhập 2. Kiểm tra các trường và button | --- | Form hiển thị đủ: Email, Password, Login | Pass / Fail | |
| TC-UI-002 | Validation email | Kiểm tra thông báo khi email không hợp lệ | 1. Nhập "abc" vào email 2. Nhập password hợp lệ 3. Nhấn Login | Email="abc" | Hiển thị lỗi "Email không hợp lệ" | Pass / Fail | |

### 8.3 Mẫu test case E2E

| ID | Tiêu đề | Mô tả | Bước thực hiện | Dữ liệu test | Kỳ vọng | Kết quả | Ghi chú |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TC-E2E-001 | Đăng nhập và tạo đơn hàng | Kiểm tra luồng từ đăng nhập đến hoàn tất đơn hàng | 1. Đăng nhập 2. Chọn sản phẩm 3. Thanh toán 4. Kiểm tra đơn hàng | user, sản phẩm, thông tin thanh toán | Đơn hàng tạo thành công, trạng thái "Đã đặt" | Pass / Fail | |

---

## 9. Mẫu báo cáo kiểm thử

### 9.1 Thông tin chung

- Dự án:
- Sprint/Chu kỳ:
- Người thực hiện:
- Ngày thực hiện:
- Môi trường test:
- Phiên bản build:

### 9.2 Tóm tắt kết quả

- Tổng số test case: X
- Đã thực hiện: Y
- Pass: A
- Fail: B
- Blocked: C
- Skip: D

### 9.3 Kết quả chi tiết

| ID | Loại | Tiêu đề | Kết quả | Ghi chú |
| --- | --- | --- | --- | --- |
| TC-API-001 | API | Đăng nhập thành công | Pass | |
| TC-UI-002 | UI | Validation email | Fail | Lỗi thông báo không hiện |

### 9.4 Danh sách lỗi

| Mã lỗi | Mức độ | Mô tả | Bước lặp lại | Trạng thái |
| --- | --- | --- | --- | --- |
| BUG-001 | High | Không trả về token khi đăng nhập thành công | Gọi POST /api/auth/login | Open |
| BUG-002 | Medium | Label button bị lệch trên mobile | Mở trang login trên mobile | Open |

### 9.5 Kết luận và đề xuất

- Kết luận chung về chất lượng bản build.
- Những khu vực cần chú ý: API, UI, E2E.
- Đề xuất hành động: Fix bug, chạy regression, test lại.

---

## 10. Best Practices

- Test sớm và liên tục.
- Ưu tiên kiểm thử API trước khi kiểm thử UI.
- Sử dụng automation cho các kịch bản lặp lại.
- Chỉ dùng E2E cho các luồng nghiệp vụ quan trọng.
- Ghi lại đầy đủ bằng chứng và kết quả test.

---

## 11. Kết luận

Quy trình kiểm thử cần thực hiện tuần tự và lặp: API → UX/UI → E2E. Nội dung test case và report phải rõ ràng để dễ theo dõi và tái sử dụng.

