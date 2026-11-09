# Quy tắc làm việc chung với các AI

> File quy tắc dùng chung cho mọi dự án. Chủ dự án đưa file này cho AI đọc trước khi giao việc — AI nào cũng phải tuân theo. Quy tắc từ chủ dự án có ưu tiên cao nhất; khi quy tắc này mâu thuẫn với quy ước riêng của dự án, làm theo quy tắc này trừ khi chủ dự án nói khác.
>
> Người tạo: Sophia (Owner) — 2026-11-09. Khi có quy tắc mới, cập nhật file này và đăng lên chỗ chung để các AI khác cùng biết.

## 1. Vai trò

- **Chủ dự án (Owner)** quyết định cuối cùng: giao việc, duyệt merge, đóng issue/PR. AI không tự quyết những việc này.
- **AI** chỉ làm việc trong phạm vi được giao, báo cáo đầy đủ, và dừng lại hỏi khi không chắc.

## 2. Một AI – một nhiệm vụ, một nhánh

- Mỗi AI chỉ làm **một nhiệm vụ, trên một nhánh của chính mình**. Chỉ đọc, review, commit, push nhánh mình phụ trách.
- **Không tự truy cập nhánh của AI khác**, kể cả để tìm lỗi, so sánh hay giải quyết xung đột. Phát hiện lỗi hoặc xung đột liên quan nhánh khác: **dừng ở ranh giới, báo chủ dự án** kèm vị trí cụ thể, chờ phân công. Không tự đọc nhánh kia, không cherry-pick, không tự merge/rebase, không sửa hộ.
- Không tự thay đổi branch protection, workflow/CI, quyền truy cập, hay bật tác vụ CI/build nặng.
- AI yếu hơn (model nhỏ) thì giới hạn code chặt hơn: chỉ sửa file trong phạm vi task; không xóa/đổi tên file; không sửa CI/workflow; không dùng lệnh phá hoại (xóa, dọn, reset DB); mọi thay đổi qua đủ vòng review, diff nhỏ từng bước; không chắc thì hỏi chủ dự án, không đoán.

## 3. Ít nhất BA vòng review toàn diện, tuần tự

> ⏸ **TẠM ĐÓNG từ 2026-11-09 theo quyết định của Owner:** các AI khác tạm thời KHÔNG tự làm 3 vòng review (tốn token) — chỉ code; việc review Owner **giao riêng cho Muse** (hoặc AI khác do Owner chỉ định từng việc). Nội dung các vòng dưới đây giữ nguyên để dùng khi mở lại.

- Một vòng review = rà soát toàn bộ phạm vi nhánh ở **đủ 6 khía cạnh**: (1) đúng chức năng/nghiệp vụ và UX; (2) lỗi/độ ổn định (bất đồng bộ, race, hủy tác vụ, timeout, mất dữ liệu); (3) hiệu năng/tài nguyên; (4) code sạch/khả năng bảo trì; (5) bảo mật/vận hành; (6) test/hồi quy.
- Mỗi vòng: review → sửa → kiểm chứng lại → ghi nhật ký. Vòng sau review **bản sau vòng trước**. Không gộp, không chia một đợt đọc thành nhiều vòng cho đủ chỉ tiêu.
- Nếu còn lỗi đáng kể thì tiếp tục vòng 4, 5... Không tự tuyên bố đạt chỉ vì đủ ba lượt. Không tự gắn trạng thái "được merge".

## 4. Nhật ký review và báo cáo ngắn gọn

- Sau mỗi vòng, báo cáo tối thiểu: nhánh nào, SHA nào, lỗi gì, **vòng thứ mấy, thời gian chạy từng lần**, kết quả đủ 6 khía cạnh, commit sửa, test đã chạy và kết quả (hoặc ghi rõ chưa chạy và vì sao), rủi ro còn lại, trạng thái (`Đang review` / `Chưa đạt` / `Đủ điều kiện trình chủ dự án`).
- Báo cáo ngắn gọn, đủ ý, không lan man. Chỉ ghi lỗi mới, không viết lại báo cáo vòng trước.
- Review xong có lỗi thì **phải báo để còn sửa**, không chỉ kết luận đạt.

## 5. Merge — chỉ khi chủ dự án xác nhận

- Không push trực tiếp vào `main`. Không tự merge, squash, rebase, đóng PR/issue.
- Chỉ merge khi chủ dự án **xác nhận rõ ràng cho PR cụ thể đó**, sau khi đọc báo cáo review/test. Qua đủ vòng review không đồng nghĩa được phép merge.
- PR bị conflict với nhánh AI khác: không tự hòa giải, báo chủ dự án.
- Nhánh đã merge vào main thì có thể xóa.
- Mỗi lần merge vào main phải **báo cho chủ dự án ngay**.

## 6. Issue / nhánh / PR — cách các AI thảo luận và làm việc

- **Issue = phòng thảo luận của các AI.** Cần bàn trước khi làm thì mở issue. AI nào review xong, thấy lỗi thì đăng lỗi đó vào cùng issue đó (ví dụ 3 AI cùng review 1 lúc, mỗi người đưa lỗi của mình lên), rồi mọi người cùng thảo luận trong issue — mọi ý kiến gom một chỗ, không rải rác.
- **Nhánh fix = chỗ làm việc.** Mỗi lần tạo nhánh fix, trong nhánh phải có sẵn: lỗi là gì + phương án xử lý thế nào. Nhánh đó luôn đi kèm 1 PR xin gộp vào main.
- **Không tạo nhánh thì tùy từng việc** mà quyết có mở issue không. Việc nhỏ thì chỉ nói trong chat, khỏi mở issue hay nhánh.
- Issue nằm độc lập — không thuộc nhánh nào hay main; nó là phiếu giao việc, có thể trỏ tới nhánh/file liên quan. PR là đơn xin gộp — gắn với một nhánh cụ thể, liệt kê đúng những gì đã đổi, là nơi review trước khi gộp.

## 7. Sửa lỗi tận gốc, không hardcode

- Sửa lỗi phải sửa **tận gốc**, không chỉ sửa thông báo hay triệu chứng.
- Không fix hardcode — cách sửa phải tổng quát, không gắn với ví dụ test cụ thể hay giá trị chỉ đúng với trường hợp đó.
- Một ví dụ chưa chắc đã ổn: kiểm chứng qua nhiều vòng, nhiều trường hợp khác nhau mới kết luận.

## 8. Thứ tự làm việc

- Làm **nhóm rủi ro thấp trước**; các nhóm lỗi khác bỏ qua, tính sau.
- Fix xong rồi mới phát triển tiếp mục chưa làm.

## 9. Tiết kiệm token

- **Báo cáo tiết kiệm token:** chỉ ghi nhánh, SHA, thời gian chạy, kết quả (chặn/nên sửa/nhỏ), mỗi mục kiểm chứng 1 dòng kèm bằng chứng, chỉ ghi lỗi mới.
- **Code tiết kiệm token:** file nhỏ, tập trung một nhiệm vụ; không lặp code, không để code chết; tên rõ nghĩa; comment ngắn, chỉ ghi chỗ khó hiểu; không viết dài dòng, không lồng nhau quá sâu; code liên quan để gần nhau; giữ mẫu nhất quán.

## 10. Việc được duyệt sẵn / việc cần xác nhận riêng

- **Được duyệt sẵn, không cần hỏi:** mọi lệnh đọc; đăng bình luận/trả lời trên GitHub; đăng đánh giá (review PR); lệnh đánh giá, kiểm tra code (kể cả giao agent review).
- **Cần chủ dự án xác nhận riêng từng việc:** merge vào main, đóng issue/PR, sửa code trên main.
- Khi được giao sửa thì **tự làm tiếp đến cùng**, đừng hỏi chấp nhận từng bước — chỉ hỏi khi việc đó quan trọng.

## 11. Cách liên lạc với chủ dự án

- Chủ dự án **chỉ đọc chat, không đọc GitHub**. Mọi việc trên GitHub (comment, review, quyết định, PR) đều phải được tóm tắt lại trong chat, bằng tiếng Việt dễ hiểu.
- Hạn chế làm phiền bằng thẻ duyệt lặp lại: ưu tiên cách làm không hiện thẻ (ví dụ dùng git CLI thay cho connector khi đọc).

## 12. Quy ước đặt tên nhánh (điều chỉnh theo dự án)

- Tên nhánh gồm: giai đoạn → ai làm → tính năng + ngày. Nhánh sửa lỗi thêm `fix` trong tên. Ví dụ: `giai-doan-2/muse/fix-ten-loi-2026-11-09`.

## 13. Bảo mật trong review

- Trong review, **bỏ qua lỗi bảo mật** trừ khi chủ dự án yêu cầu kiểm tra — tập trung vào đúng chức năng, ổn định, hiệu năng.
