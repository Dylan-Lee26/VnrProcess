# Hướng dẫn định nghĩa và estimate Story Point hiệu quả

## 1. Mục đích

Mục tiêu của tài liệu này là giúp team:
- Hiểu rõ Story Point là gì và cách dùng đúng bản chất.
- Chuẩn hóa quy trình estimate story point.
- Giảm bias, giảm rủi ro, tránh tranh luận không cần thiết.
- Tạo nền tảng so sánh nhất quán giữa các user story.

## 2. Story Point là gì?

Story Point là đơn vị đo tương đối dùng để đánh giá:
- Độ lớn của công việc.
- Độ khó kỹ thuật.
- Độ phức tạp.
- Mức rủi ro / bất định.

Lưu ý quan trọng:
- Story Point không phải thời gian (không phải giờ, ngày, tuần).
- Không dùng để estimate trực tiếp theo tốc độ hoàn thành.
- Dùng để so sánh relative giữa các user story.

## 3. Nguyên tắc estimate

- Dựa trên thông tin rõ ràng: user story, acceptance criteria, business flow.
- Estimate độc lập: mỗi người có thể ước lượng riêng trước khi thảo luận.
- Dùng anchor/reference để cân đối: so với baseline đã xác định.
- Không estimate theo giờ.
- Story nhỏ nên nằm trong khoảng <= 13 point.
- Sau sprint đánh giá accuracy để cải tiến.

## 4. Các cách ước lượng Story Point

### 4.1. T-shirt size

Dùng size áo để xác định độ lớn ở mức high-level:
- Extra Small = 1 điểm
- Small = 2 điểm
- Medium = 3 điểm
- Large = 4 điểm
- Extra Large = 5 điểm

### 4.2. Lũy thừa 2

Dãy 1, 2, 4, 8, 16 giúp team ước lượng theo biên độ rộng hơn khi khó đoán.

### 4.3. Fibonacci cho Story Point

Dãy Fibonacci thường dùng khi muốn phân biệt rõ ràng giữa các mức độ khác nhau:
- 1, 2, 3, 5, 8, 13, 20, 40, 100

## 5. Baseline và anchor

Team nên chọn một hoặc vài user story làm baseline (tham chiếu):
- Tham chiếu đọc (Read) = 2 điểm
- Create/Update/Delete = 3 điểm
- Tech/Logic phức tạp hơn = 5 điểm

Khi estimate, mỗi story nên so sánh với baseline này để giữ consistency.

## 6. Checklist estimate trước khi ước lượng

| Giai đoạn | Nhóm tiêu chí | Checklist chi tiết | Mục đích |
|---|---|---|---|
| Trước estimate | Requirement | User story rõ, không ambiguous; có business flow | Tránh hiểu sai |
| Trước estimate | Acceptance Criteria | Có AC; Success + fail case; Edge case | Đảm bảo test được |
| Trước estimate | Technical | Có solution; Biết impacted system; Không blocker | Giảm rủi ro |
| Trong estimate | Planning Poker | Estimate độc lập; reveal cùng lúc | Tránh bias |
| Trong estimate | Anchor | So với baseline 1-3-5 | Consistency |
| Validate | Anti-time | Không estimate theo thời gian | Đúng bản chất SP |
| Validate | Size | Story <= 13 SP | Dễ kiểm soát |
| Sau sprint | Accuracy | Check over/under estimate | Cải thiện |
| Anti-gaming | Inflate | Velocity tăng bất thường | Detect gian lận |

## 7. Quy trình estimate story point hiệu quả

### Bước 1: BA chuẩn bị

- BA phải làm rõ user story và acceptance criteria.
- Nếu requirement chưa rõ, phải refine lại trước khi estimate.
- Nên có business flow và các tình huống chính.

### Bước 2: Xác định baseline

- Chọn một câu chuyện tham chiếu làm base story.
- Base story phải rõ ràng, hoàn thành được, có DoD.
- Dùng base story để so sánh các story khác.

### Bước 3: Thực hiện estimate bằng Planning Poker

- Mỗi thành viên nhận bộ thẻ estimate.
- Tất cả thành viên chọn backlog item, thảo luận tính năng và đặt câu hỏi.
- Khi tính năng được thảo luận đủ, mỗi người đưa ra con số estimate riêng.
- Reveal cùng lúc: nếu giống nhau thì assign điểm ngay.
- Nếu khác nhau, thảo luận tiếp để nhất trí.

### Bước 4: Kiểm tra của team

- Nếu có quá nhiều ước lượng khác nhau, hãy thảo luận về giả định.
- So sánh với baseline và với các story đã hoàn thành.
- Nếu cần, refine lại story hoặc acceptance criteria.

### Bước 5: Assign Story Point

- Khi team đồng thuận, gán story point cho user story.
- Nếu không đồng thuận, đưa về BA refine lại hoặc thảo luận tiếp.

## 8. Công cụ support

- Planning Poker: để reveal cùng lúc và tránh bias.
- Anchor: dùng reference story để giữ consistency.
- Velocity Sprint: dùng để tham khảo tốc độ, không phải để đổi SP thành ngày.

## 9. Lưu ý khi dùng Velocity

Velocity là tốc độ thực tế của sprint trước đó, ví dụ:
- Sprint trước: 18 điểm.
- Sprint hiện tại: 45 ngày công.
- Nếu hệ số tập trung là 40%, tốc độ ước tính = 50 ngày công x 40% = 20 điểm.

> Quan trọng: không dùng velocity để biến story point thành ngày công cho từng story.

## 10. Những quy tắc vàng

- Story point là relative, không đo bằng thời gian.
- Estimate độc lập trước khi thảo luận.
- Không để một người áp đảo quyết định toàn bộ.
- Story point của một story cần được team thỏa thuận.
- Regular review: sau mỗi sprint, so sánh estimate và thực tế để điều chỉnh.

## 11. Kết luận

Để team estimate story point hiệu quả, cần:
- Chuẩn bị kỹ trước khi estimate.
- Có baseline và anchor rõ ràng.
- Thực hiện Planning Poker để tránh bias.
- Giữ story nhỏ và dễ quản lý.
- Đánh giá kết quả sau sprint để cải tiến liên tục.

*Với quy trình và checklist này, team sẽ estimate story point một cách nhất quán và dễ kiểm soát hơn.*