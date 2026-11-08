# Ý kiến của Muse — Giao tiếp giữa các AI trong dự án

- Tác giả: Muse (Muse AI, Assistant G1)
- Ngày: 2026-11-08
- Dự án: VietVoice AI Studio (repo `dvphuong20786/video-ai`)
- Bối cảnh: nhiều AI cùng làm việc trong một dự án, các AI được cấp quyền làm việc trên GitHub. Ý tưởng là tạo một nơi chung để các AI giao tiếp với nhau: một AI review code nhánh khác, thấy điểm không ổn thì đăng lên nhóm chung; AI phụ trách nhánh đó đọc được, tự review đối chiếu, phân định ý kiến nào đúng, ý kiến nào không nên làm và vì sao, rồi sửa hoặc bác bỏ sao cho các AI khác thấy đúng và đồng ý. Trước khi làm, mỗi AI đưa ra một kế hoạch, tham khảo nhau, chọn phương án tốt nhất rồi thống nhất cùng nhau làm.

## Đánh giá

Nên làm. Đây chính là cách nhóm dev người vẫn làm việc: review chéo qua pull request và bàn kế hoạch trước khi code. Lợi nhất là bắt lỗi sớm, chọn được phương án tốt trước khi tốn công làm, và người điều phối (Sophia) bớt phải làm "tổng đài" chuyển lời giữa các AI. Điều kiện bắt buộc: phải giữ được quy tắc 1 AI 1 nhánh — giao tiếp chỉ được phép ở mức đọc và bình luận, không được sửa code trên nhánh của AI khác.

## Hướng làm đề xuất: lấy GitHub làm "phòng chung"

Vì sao không phải chat: các AI thuộc hệ thống khác nhau (Muse, Claude, Antigravity...), mỗi AI sống trong môi trường riêng, không có một phòng chat chung nào mà tất cả đều vào được trừ khi xây cầu nối cho từng hệ thống (tốn công, dễ hỏng); chat trôi nhanh, khó gắn một ý kiến vào đúng dòng code, khó tìm lại quyết định cũ. GitHub là nơi duy nhất tất cả AI đều đã có quyền và đều đang làm việc: ý kiến gắn thẳng vào code, lưu vết vĩnh viễn, tìm lại được.

## Cách tổ chức trên GitHub

- **PR review là nơi review chéo:** AI review đăng review chính thức trên PR của nhánh kia, bình luận theo từng dòng code kèm tóm tắt. AI chủ nhánh trả lời từng ý ngay trong thread: đồng ý thì sửa, không đồng ý thì nêu lý do. Các AI khác đọc được toàn bộ thread và thấy ai đúng. Thread chỉ đóng khi sửa xong hoặc bên review đồng ý với lý do bác bỏ.
- **Issue lập kế hoạch là nơi bàn phương án trước khi làm:** mỗi task có 1 issue kế hoạch. Trước khi code, AI được giao (hoặc nhiều AI) đăng phương án của mình dưới dạng comment; các AI khác phản biện; chốt 1 phương án, ghi vào nội dung issue, rồi mới bắt đầu code.
- **Issue điều phối (ghim) là bảng tin chung:** 1 issue ghim trong repo liệt kê nhánh đang làm, AI phụ trách, trạng thái, link tới issue kế hoạch và PR tương ứng. Mỗi AI trước khi bắt đầu việc phải đọc issue này.
- **Nhật ký quyết định trong repo** (ví dụ `docs/quyet-dinh.md`): ghi lại các quyết định đã thống nhất kèm lý do, để AI mới vào không tranh luận lại chuyện cũ.
- **Chat (nếu muốn)** chỉ dùng để báo nhanh ("tôi vừa đăng review lên PR #X"), không phải nơi quyết định. Quyết định chỉ tính khi đã ghi lên GitHub.

## Quy trình cụ thể

- **Review chéo:** AI review đăng ý kiến ngắn gọn, mỗi ý 1 dòng kèm file:dòng làm bằng chứng. AI chủ nhánh phải trả lời từng ý, không được lặng lẽ bỏ qua. Tối đa 2 vòng qua lại, sau đó Sophia làm trọng tài. Sophia vẫn xác nhận merge như quy tắc hiện tại.
- **Lập kế hoạch:** đăng phương án, các AI khác có 1 lượt góp ý ngắn gọn, chốt phương án tốt nhất rồi làm theo. Đổi phương án giữa chừng phải quay lại issue và thống nhất lại.

## Công nghệ cần

Sẵn có, không cần mua hay xây thêm — GitHub Issues, Pull Request Reviews, tùy chọn thêm GitHub Discussions hoặc Projects.

Vấn đề cần giải quyết: hiện tại AI dùng GitHub connector thì mỗi lần đọc/ghi đều hiện thẻ duyệt cho Sophia. Đăng comment qua connector cũng sẽ hiện thẻ. Cách giải quyết: cấp cho mỗi AI một token GitHub riêng (fine-grained PAT, chỉ quyền đọc/ghi issue và PR review trên repo này), lưu trong kho bảo mật, AI dùng API hoặc gh CLI trực tiếp thì không hiện thẻ (tương tự cách đã làm với deploy key cho git push). Không cần thêm máy chủ, bot hay ứng dụng chat mới cho giai đoạn đầu; chỉ thêm chat khi thấy thực sự cần tốc độ.

## Quy tắc an toàn

- Chỉ bình luận, không sửa nhánh người khác.
- AI chủ nhánh là người duy nhất commit lên nhánh mình.
- AI chủ nhánh phải trả lời từng ý review (sửa hoặc bác bỏ có lý do).
- Sophia là trọng tài cuối cùng khi các AI không thống nhất và vẫn xác nhận merge.
- Bình luận ngắn gọn, tiết kiệm token.
- Quyết định đã thống nhất phải ghi lại, không để trong đầu AI nào.

## Các bước triển khai (thử nhỏ trước)

1. Tạo issue điều phối ghim và 1 issue kế hoạch cho Task 3 (đang làm) để thử luồng bàn kế hoạch.
2. Thử 1 vòng review chéo qua PR review (thay vì báo cáo qua Sophia) trên nhánh Task 3.
3. Cấp token GitHub riêng cho các AI để hết thẻ duyệt khi comment.
4. Nếu chạy tốt 1–2 task, ghi quy tắc này vào file agent (agents-sub.md) để mọi AI đều biết; nếu không tốt, quay lại cách cũ (qua Sophia).

## Rủi ro và lưu ý

- Tranh luận kéo dài: giới hạn 2 vòng qua lại, sau đó Sophia quyết.
- Comment dài tốn token: bắt buộc ngắn gọn.
- AI khác hệ thống không chung chat được: vì vậy GitHub là đáp án, chat chỉ là phụ.
- Token GitHub của từng AI phải giới hạn quyền (chỉ repo này, chỉ issue và PR) và lưu trong kho bảo mật, không để trong code hay chat.
