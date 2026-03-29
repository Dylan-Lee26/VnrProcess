# Vận hành Sprint 2 Tuần

## 1. Mục tiêu của sprint 2 tuần
Sprint 2 tuần cần tập trung vào việc cân bằng giữa tiến độ, chất lượng và phản hồi sớm. Mục tiêu của sprint nên được diễn giải thành các kết quả cụ thể, đo lường được.

- Hoàn thành các công việc đã cam kết trong sprint backlog theo phạm vi nhất định.
- Đảm bảo chất lượng trình độ cao thông qua kiểm thử tự động, kiểm thử thủ công và deploy lên môi trường staging.
- Giảm thiểu rủi ro bằng cách phát hiện vấn đề càng sớm càng tốt, đánh giá giữa kỳ và điều chỉnh ngay trong sprint.
- Chuẩn bị sản phẩm để demo rõ ràng, thu thập phản hồi từ PO và stakehoders, sau đó rút kinh nghiệm cho sprint sau.
- Xây dựng thói quen liên tục cải tiến quy trình làm việc, phản hồi và phối hợp đội nhóm.

## 2. Thành phần chính của sprint
Sprint 2 tuần gồm 6 phần chính: lập kế hoạch, làm việc hàng ngày, phát triển và kiểm thử liên tục, deploy staging, demo/review và retrospective.

### 2.1. Sprint Planning
Sprint Planning là phiên họp đầu sprint để thống nhất mục tiêu và định hướng.

- Thời điểm: trước khi sprint bắt đầu và nên được thực hiện vào ngày đầu tiên của sprint.
- Người tham gia:
  - Product Owner (PO)
  - Engineering Manager / Scrum Master (EM)
  - Developers (SE)
  - Quality Control (QC)
  - Các thành viên kỹ thuật liên quan và bất kỳ stakeholder thiết yếu nào.
- Công việc:
  - Xác định mục tiêu sprint (Sprint Goal): mô tả rõ ràng giá trị cần đạt được và lý do.
  - Chọn backlog item đủ rõ, đủ điều kiện (ready), có xác suất hoàn thành cao.
  - Ước lượng effort bằng story points.
  - Phân chia task theo ngày, xác định owner và phụ thuộc giữa các task.
  - Xác định tiêu chí hoàn thành (Definition of Done) cho mỗi item: code, test, review, deploy, tài liệu, chấp nhận PO.
  - Kiểm tra rủi ro và giả định: những phần chưa chắc chắn, cần research, dependency bên ngoài.

#### Ví dụ nội dung Sprint Planning
- Sprint Goal: "Hiển thị và cập nhật được tiến độ mục tiêu và phiếu mục tiêu".
- Backlog item: Danh sach phiếu mục tiêu đã được duyệt, thông báo lỗi đầu vào, UI/UX giao diện đăng ký, test case.
- Nguồn lực: 1 dev frontend, 1 dev backend, 1 tester.
- Ước lượng: 20 story points, dự phòng 2 ngày cho xử lý lỗi và review.

### 2.2. Daily Meeting
Daily Meeting là buổi họp nhanh mỗi ngày để đồng bộ và gỡ tắc.

- Thời lượng: 10-15 phút mỗi ngày, tổ chức cố định vào buổi sáng.
- Nội dung chính:
  - Hôm qua tôi đã làm gì?
  - Hôm nay tôi định làm gì?
  - Có chặn/khó khăn gì không?
- Mục đích:
  - Đồng bộ tiến độ, giúp team hiểu trạng thái chung.
  - Phát hiện vấn đề sớm và điều phối xử lý nhanh.
  - Đảm bảo không ai làm việc độc lập mà thiếu liên kết.
- Lưu ý:
  - Không đi sâu vào giải quyết kỹ thuật trong daily meeting.
  - Nếu cần thảo luận chi tiết, chuyển thành separate session sau.
  - Ghi nhận blocker và gán trách nhiệm follow-up.

### 2.3. Phát triển và kiểm thử liên tục
Phát triển và kiểm thử nên chạy song song theo mô hình Agile, không phải xong dev mới có test.

- SE:
  - Phân tích yêu cầu, triển khai tính năng theo chuẩn code style.
  - Cung cấp đầy đủ thông tin cho mỗi User Story: Endpoint API, URL web, Payload mẫu, response mẫu và các điều kiện nghiệp vụ liên quan.
  - Viết unit test, integration test khi cần.
  - Commit thường xuyên, giữ branch nhỏ gọn.
  - Thực hiện refactor khi cần để giảm technical debt.
  - Tham gia code review, nhận và sửa feedback.
- QC:
  - Chuẩn bị testcase và test plan dựa trên acceptance criteria.
  - Cung cấp đầy đủ testcase cho mỗi User Story, kèm theo bước kiểm thử, dữ liệu đầu vào, kết quả mong đợi và mức độ ưu tiên.
  - Kiểm tra sớm các chức năng đã hoàn thiện.
  - Test regression để bảo đảm không ảnh hưởng chức năng cũ.
  - Báo lỗi rõ ràng: bước tái tạo, kết quả mong đợi, kết quả thực tế, ảnh chụp màn hình/log, video hoặc evidence liên quan.
- EM:
  - Giám sát tiến độ sprint, duy trì bảng task.
  - Điều phối tài nguyên khi có chặn.
  - Hỗ trợ team liên hệ với PO/stakeholder để làm rõ yêu cầu.
  - Đảm bảo tài nguyên deploy và môi trường staging sẵn sàng.
- Song song thực hiện:
  - Tích hợp liên tục: build và kiểm tra tự động mỗi khi merge code.
  - Chuẩn bị môi trường staging ổn định để test và demo.
  - Kiểm tra chất lượng đầu vào trước deploy: compile, unit test pass, đánh giá code review, coverage.

#### Mô hình làm việc hàng ngày
- Phân chia task nhỏ: mỗi task không quá 1-2 ngày.
- Kết hợp pair programming hoặc code review nhanh để giảm lỗi.
- Xác định task có dependency và xử lý ưu tiên.
- Cập nhật trạng thái board (kanban/scrum) hàng ngày.

### 2.4. Deploy Staging
Deploy staging là bước quan trọng để đánh giá tính năng trong môi trường gần thực tế.

- Mốc deploy chính:
  - Giữa sprint: đánh giá giữa kỳ, kiểm tra tổng thể các tính năng đang được phát triển.
  - Cuối sprint: chuẩn bị demo và kiểm soát chất lượng trước khi kết thúc sprint.
- Mục tiêu deploy staging:
  - Kiểm tra tính năng trên môi trường tương tự production.
  - Thực hiện kiểm thử tích hợp giữa các module và dịch vụ.
  - Kiểm thử hồi quy các phần đã hoàn thành.
  - Phát hiện lỗi môi trường, cấu hình, dữ liệu và giao diện.
- Ai tham gia:
  - SE: chuẩn bị và chạy deployment script, kiểm tra logs, fix lỗi deploy.
  - QC: thực hiện test trên staging, xác nhận bug và verify fix.
  - EM: điều phối, kiểm soát thời gian deploy, ghi nhận kết quả.
- Quy trình:
  - Chuẩn bị release note, xác định scope deploy.
  - Backup dữ liệu staging nếu cần.
  - Thực hiện deploy theo checklist kỹ thuật.
  - Kiểm tra nhanh sau deploy (smoke test) trước khi mở rộng test.
  - Ghi nhận issue và xử lý trong sprint.

### 2.5. Demo & Review
Demo giúp PO và team cùng nhìn thấy kết quả sprint cụ thể.

- Thời điểm: cuối sprint, hoặc giữa sprint nếu cần.
- Người tham gia: PO, nhóm dev, QC, EM, và các stakeholder liên quan.
- Nội dung:
  - Trình diễn các tính năng hoàn thành theo Sprint Goal.
  - Nhấn mạnh kết quả đạt được và giới hạn hiện tại.
  - Đánh giá mức độ đạt mục tiêu, xác nhận phần nào đạt "Done".
  - Thu thập phản hồi thực tế của PO và stakeholders.
- Kết quả:
  - Danh sách feature đã hoàn thành được PO chấp nhận.
  - Những hạng mục chưa hoàn tất cần xử lý tiếp theo.
  - Ghi nhận cải tiến UX, chức năng, performance nếu có.

#### Điểm cần lưu ý khi demo
- Chuẩn bị kịch bản demo trước.
- Sử dụng dữ liệu thực tế hoặc mẫu giống thực tế.
- Trình bày rõ ràng: mục tiêu, ảnh hưởng, đầu vào/đầu ra.
- Ghi nhận câu hỏi và phản hồi ngay trong buổi demo.

### 2.6. Sprint Retrospective
Retrospective là nơi team học hỏi và cải tiến liên tục.

- Thời điểm: cuối sprint, tốt nhất là ngay sau demo.
- Người tham gia: PO, EM, SE, QC, Team.
- Nội dung:
  - Điều gì đã làm tốt? (process, phối hợp, kỹ thuật, test)
  - Điều gì chưa tốt? (blocker, ước lượng, deployment, quality)
  - Hành động cụ thể để cải thiện sprint sau.
- Cách tổ chức:
  - Sử dụng phương pháp Start/Stop/Continue hoặc What Went Well / What Can Be Improved.
  - Ghi lại ít nhất 2-3 hành động cải tiến có thể thực hiện ngay.
  - Phân công trách nhiệm theo dõi và hoàn thành các hành động.
- Kết quả:
  - Danh sách việc cần làm để tối ưu tiến độ, chất lượng và phối hợp.
  - Kế hoạch kiểm tra lại hiệu quả cải tiến trong sprint tiếp theo.

## 3. Lịch hoạt động điển hình cho sprint 2 tuần (10 ngày làm việc)
Một sprint 2 tuần thường có 10 ngày làm việc, chia theo 3 giai đoạn chính: bắt đầu, giữa, hoàn thiện.

### Ngày 1–5: Dev + Test
- SE:
  - Bắt đầu phát triển tính năng theo backlog đã chọn.
  - Triển khai backend, frontend, API, database và các thành phần liên quan.
  - Viết unit test, kiểm tra cục bộ, chuẩn bị branch để review.
- QC:
  - Viết testcase và test plan cho mỗi user story.
  - Thực hiện test trên môi trường dev/staging nội bộ.
  - Báo lỗi sớm, đồng thời xác thực fix nhanh.
- EM:
  - Theo dõi tiến độ hàng ngày và điều chỉnh tài nguyên.
  - Hỗ trợ giải quyết chặn, phối hợp giữa dev và QC.
- Kết quả mong muốn:
  - Nhiều feature/nhóm task đầu tiên đạt trạng thái review hoặc test.
  - Bug được phát hiện và xử lý trong cùng ngày.
  - Các task đã được đánh dấu rõ trạng thái trên board.

### Ngày 5: Deploy Staging giữa sprint
- Hoàn tất các công việc đủ điều kiện deploy lên môi trường staging.
- Thực hiện deploy bằng pipeline CI/CD.
- QC thực hiện smoke test, test các tính năng mới và những phần phụ thuộc.
- SE khắc phục lỗi phát sinh, đặc biệt là lỗi deploy và lỗi tích hợp.
- EM đánh giá xem sprint có cần điều chỉnh scope hay ưu tiên lại.

### Ngày 6–9: Hoàn thiện + Fix + Chuẩn bị deploy cuối
- Tiếp tục phát triển các task còn lại và hoàn thiện phần chưa xong.
- Fix lỗi do QC báo, verify ngay trên staging.
- Hoàn thiện tài liệu, ghi chú kỹ thuật, release note nội bộ.
- Chuẩn bị nội dung demo: kịch bản, dữ liệu mẫu, ghi chú các điểm cần trình bày.
- Ngày 9: Deploy staging cuối cùng, kiểm tra tổng thể và xác nhận không có lỗi nghiêm trọng.

### Ngày 10: Tổng kết Sprint
- Thực hiện demo với PO và stakeholders.
- Thu thập phản hồi chi tiết và xác nhận kết quả sprint.
- Sprint retrospective để đánh giá process.
- Xây dựng kế hoạch cải tiến cho sprint tiếp theo, bao gồm cả việc cải tiến flow, công cụ và quản lý backlog.

## 4. Vai trò và trách nhiệm chi tiết
Mỗi thành viên có vai trò rõ ràng nhưng cần phối hợp chặt chẽ.

### Product Owner (PO)
- Xác định yêu cầu, giá trị ưu tiên và backlog item.
- Chọn backlog item phù hợp với sprint goal, đảm bảo đủ điều kiện ready.
- Giải thích acceptance criteria rõ ràng cho team.
- Duyệt kết quả demo và chấp nhận sản phẩm khi đạt yêu cầu.
- Hỗ trợ làm rõ yêu cầu, trả lời câu hỏi kịp thời.
- Theo dõi sự phù hợp giữa kết quả và nhu cầu kinh doanh.

### Software Engineer (SE)
- Phát triển tính năng và fix bug theo standard team.
- Viết code sạch, dễ bảo trì, tuân thủ chuẩn coding và best practice.
- Cung cấp đầy đủ thông tin cho mỗi User Story: Endpoint API, URL web, Payload/ngữ cảnh request, response mẫu, điều kiện tiền đề và kết quả kỳ vọng.
- Thực hiện unit test, nếu cần thì sẽ thực hiện integration test, và tham gia code review.
- Sử dụng branch nhỏ, commit rõ ràng, giữ lịch sử thay đổi minh bạch.
- Hỗ trợ deploy, kiểm tra build và xử lý vấn đề môi trường.
- Ghi chú rõ ràng các thay đổi để hỗ trợ QC và PO đánh giá.

### Quality Control (QC)
- Chuẩn bị test plan, test case, kịch bản kiểm thử.
- Cung cấp đầy đủ testcase cho mỗi User Story, bao gồm bước kiểm thử, dữ liệu vào, kết quả mong đợi, kết quả thực tế và mức độ ưu tiên.
- Thu thập evidence kiểm thử rõ ràng: screenshot, log, video, link issue tracker.
- Thực hiện kiểm thử chức năng, UI.
- Báo lỗi rõ ràng, kèm theo bước tái tạo và mức độ ưu tiên.
- Theo dõi fix, verify lại kết quả trên môi trường phù hợp.
- Đảm bảo kết quả test được lưu trữ và có thể truy xuất.
- Hỗ trợ xác nhận chất lượng trước khi deploy staging.

### Engineering Manager (EM)
- Giám sát tiến độ sprint, đảm bảo team bám sát kế hoạch.
- Điều phối tài nguyên, giải quyết blocker và duy trì tinh thần team.
- Duy trì thông suốt daily meeting, review và retrospective.
- Thúc đẩy việc cải tiến quy trình và loại bỏ waste.
- Hỗ trợ xây dựng môi trường làm việc hiệu quả: công cụ, CI/CD, staging.

### Team
- Thực hiện công việc theo kế hoạch và chia sẻ tiến độ.
- Tham gia chia sẻ, hỗ trợ lẫn nhau trong quá trình làm việc.
- Góp ý và cải thiện quy trình làm việc liên tục.
- Tham gia demo, retrospective và chuẩn bị kế hoạch cho sprint sau.
- Đảm bảo tính minh bạch về trạng thái công việc trên board.

## 5. Quy tắc vận hành quan trọng
Các quy tắc giúp sprint vận hành ổn định và hiệu quả.

- Chỉ đưa vào sprint những công việc rõ ràng và có khả năng hoàn thành trong 10 ngày.
- Sprint goal cần được mọi người hiểu, đồng thuận và gắn với giá trị sản phẩm.
- Definition of Done cần tối thiểu:
  - Code hoàn thiện, build thành công và đã review.
  - Test case đã viết và thực thi, kết quả test được lưu.
  - Deploy lên staging thành công và không có lỗi nghiêm trọng.
  - PO chấp nhận sản phẩm theo acceptance criteria.
  - Tài liệu/ghi chú cần thiết đã được cập nhật.
- Nếu phát hiện lỗi nghiêm trọng, ưu tiên fix sớm và deploy lại càng nhanh càng tốt.
- Không kéo công việc sang sprint tiếp theo nếu chưa được đánh giá lại và chuyển lại backlog.
- Giữ nguyên tắc "done is done": hoàn thành toàn diện, không chỉ hoàn thành code.
- Duy trì liên lạc thường xuyên với PO để tránh hiểu nhầm yêu cầu.

## 6. Checklist vận hành sprint 2 tuần
- [ ] Sprint goal rõ ràng và được team đồng thuận.
- [ ] Backlog item (US) đã ước lượng và đủ điều kiện ready.
- [ ] Daily meeting tổ chức đều đặn và ghi nhận blocker.
- [ ] Code review và unit test đã thực hiện cho mỗi feature.
- [ ] QC có testcase và test report cho các US chính.
- [ ] Deploy staging ít nhất 2 lần (giữa sprint và cuối sprint).
- [ ] Sprint retrospective đã diễn ra và có hành động cải tiến.
- [ ] Các action item của retrospective đã được ghi nhận và phân công.

## 7. Lưu ý khi áp dụng sprint 2 tuần
Sprint 2 tuần phù hợp với dự án cần phản hồi nhanh và duy trì nhịp độ làm việc ổn định.

- Sprint 2 tuần phù hợp với:
  - Dự án cần phản hồi nhanh từ PO/stakeholder.
  - Các tính năng/bug có qui mô vừa, không quá lớn.
  - Team cần giữ nhịp độ đều đặn, tránh sprint quá dài.
- Nếu công việc quá lớn, cần tách thành task nhỏ hơn và ưu tiên phần có giá trị cao.
- Nếu sprint bị quá tải, điều chỉnh ngay trong buổi review giữa kỳ để tránh mất kiểm soát.
- Đảm bảo có tương tác thường xuyên giữa SE, QC và PO.
- Luôn giữ tinh thần cải tiến liên tục qua mỗi sprint: học từ lỗi, cập nhật process, cải thiện tài liệu.
- Kiểm tra thường xuyên các yếu tố kỹ thuật: technical debt, môi trường, performance.
- Ưu tiên delivery giá trị có thể sử dụng được hơn là hoàn thiện tất cả tính năng không cần thiết.

---

*Tài liệu này mô tả quy trình chung vận hành sprint 2 tuần. Team có thể điều chỉnh chi tiết phù hợp với đặc thù dự án và tổ chức.*
