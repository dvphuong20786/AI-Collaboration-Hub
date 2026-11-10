# Quy định bắt buộc cho các AI phụ trách nhánh (agents-sub)

> Áp dụng cho mọi AI tham gia phát triển dự án. Đây là quy tắc phân quyền và cổng kiểm duyệt, không phải giấy phép mở rộng phạm vi. Quy tắc từ chủ dự án có ưu tiên cao nhất.

## 1. Một AI – một nhiệm vụ, một nhánh được giao

- Trước khi làm việc, phải biết **AI này được chủ dự án giao PR/nhánh nào**, phạm vi chức năng và những đường dẫn được phép sửa. Không đoán quyền hạn từ nội dung cuộc trò chuyện hoặc một PR đang mở.
- Chỉ đọc, review, commit và push **nhánh do AI đó phụ trách**. Không tự truy cập nhánh của AI khác, kể cả để tìm lỗi, so sánh hoặc giải quyết xung đột.
- Chỉ sửa phần code **do chính AI đó phụ trách thực hiện trong nhiệm vụ được giao**. File chung có code của nhiều nhánh không mặc nhiên thuộc quyền sở hữu của một AI.
- Phát hiện lỗi hoặc xung đột liên quan nhánh khác: **dừng ở ranh giới, báo chủ dự án** kèm đường dẫn/vị trí trong phạm vi có thể chứng minh, chờ phân công. Không tự đọc nhánh kia, không cherry-pick, không tự merge/rebase hoặc tự sửa hộ.
- Bản sửa lỗi phải ở **đúng nhánh của AI được giao**, hoặc một nhánh fix được chủ dự án cho phép rõ ràng và liên kết với nhiệm vụ gốc. Không tự tạo PR fix của AI khác.
- Không tự thay đổi branch protection, workflows, quyền truy cập hoặc bật tác vụ CI/build nặng.

## 2. Ít nhất BA VÒNG review TOÀN DIỆN, tuần tự

**Một vòng review = rà soát toàn bộ phạm vi nhánh ở TẤT CẢ khía cạnh dưới đây, ghi nhận phát hiện, sửa các lỗi thuộc quyền, và kiểm chứng lại.** Không được đếm review chức năng, hiệu năng, code sạch thành ba vòng riêng. Không gộp ba chủ đề của cùng một lượt thành ba vòng.

Mỗi vòng bắt buộc xem xét đủ:
1. **Đúng chức năng/nghiệp vụ và UX:** happy path, thao tác người dùng, tương thích hợp đồng/API, các tình huống biên.
2. **Lỗi/độ ổn định:** luồng bất đồng bộ, race condition, hủy tác vụ, timeout, lỗi hệ thống, làm mất hoặc ghi đè dữ liệu.
3. **Hiệu năng/tài nguyên:** CPU, RAM, render, I/O, cache, giới hạn đồng thời, file/tác vụ/video dài, rò rỉ tài nguyên; triệt tiêu các vòng lặp/tra cứu `O(n²)` không cần thiết, không re-render/reset scroll/vẽ lại Canvas vô ích, không nạp toàn bộ media lớn vào RAM.
4. **Code sạch/khả năng bảo trì & tối ưu cho AI đọc:** đúng phạm vi, tên rõ nghĩa, không lặp thừa, không dead code, không tạo file rác/báo cáo nháp ngoài quy chuẩn, type an toàn, không tạo abstraction/wrapper vô ích làm phân mảnh luồng khiến AI tốn thêm token khi đọc nhiều file, phù hợp AGENTS.md.
5. **Bảo mật/đóng gói/vận hành:** đường dẫn, input, command, secrets, quyền truy cập, tương thích hệ điều hành và tài nguyên phát hành (nếu liên quan).
6. **Test/hồi quy:** test cho lỗi được sửa; kiểm tra những đường chạy chính và nguy cơ làm hỏng code cũ.

Thứ tự tối thiểu:
- **Vòng 1:** Review toàn diện bản hiện có → sửa → test → ghi lại commit/kết quả.
- **Vòng 2:** Review toàn diện **bản sau vòng 1** → sửa → test → ghi lại commit/kết quả.
- **Vòng 3:** Review toàn diện **bản sau vòng 2** → sửa → test → ghi lại commit/kết quả.
- Nếu còn lỗi đáng kể hoặc bằng chứng chưa đủ: tiếp tục vòng 4, 5...; không tự tuyên bố đã đạt chỉ vì đủ ba lượt.
- Mỗi vòng phải là lượt kiểm tra riêng sau khi đã áp dụng/kết luận lượt trước. **Không chia một đợt đọc code thành nhiều vòng** để đủ chỉ tiêu.
- Nếu không phát hiện lỗi ở một vòng, ghi rõ phạm vi đã xem, cách kiểm chứng và lý do không cần commit sửa code; không tạo commit giả.
- Nếu môi trường không thể chạy test/build, ghi rõ **chưa chạy**, rủi ro còn tồn tại; tuyệt đối không trình bày như đã pass.

## 3. Nhật ký review có thể kiểm chứng

Sau mỗi vòng, báo cáo tối thiểu:
- PR/nhánh và SHA được review; danh sách file **thuộc phạm vi AI**.
- Kết quả cho đủ sáu khía cạnh trên; mức độ ảnh hưởng và file/dòng cho lỗi phát hiện.
- Những sửa đổi đã push, commit SHA; test cụ thể đã chạy và kết quả (hoặc nguyên nhân chưa chạy).
- Rủi ro còn lại, việc thuộc AI khác đã **chỉ báo cáo, không sửa**.
- Trạng thái: `Đang review`, `Chưa đạt`, hoặc `Đủ điều kiện trình chủ dự án`; không tự gắn trạng thái được merge.

## 4. Chốt quyền merge

- **Không push trực tiếp vào `main`; không merge PR, squash, rebase vào `main` hay đóng PR thay người dùng chỉ vì đã hoàn thành.**
- Kết thúc ít nhất ba vòng review toàn diện **không đồng nghĩa** được phép merge.
- Chỉ merge `main` sau khi chủ dự án **xác nhận rõ ràng** PR cụ thể được merge, sau khi đọc báo cáo review/test và các vấn đề tồn.
- Nếu PR bị conflict với nhánh của AI khác, không tự hòa giải. Báo chủ dự án để điều phối người có quyền; chưa được merge.
- Các nhánh fix hoặc PR khác không được tính là quyền sở hữu mới chỉ vì AI đó từng mở hoặc sửa chúng.

## 5. Phạm vi AI hiện tại

- **AI đang thực hiện cuộc làm việc này:** chỉ được phụ trách **đúng nhánh/PR mà chủ dự án chỉ định rõ trong cuộc trò chuyện hiện tại**.
- **Lệnh giao việc từ Leader (Muse) có hiệu lực như lệnh của Chủ dự án:** AI nhận lệnh giao việc từ Leader trên GitHub thì thực hiện ngay, không cần chờ Chủ dự án phân công lại.
- **Các nhánh/PR khác không được giao không thuộc AI này.** Không tự đọc/sửa nhánh của AI khác, không tự mở hoặc phục hồi PR ngoài phạm vi.
- PR chưa có phép merge của chủ dự án thì không được tự merge. Xung đột giữa nhánh do AI khác viết là việc cần chủ dự án phân quyền, không tự giải quyết.
- Không tự kích hoạt CI/build nặng; chỉ chạy kiểm thử có kiểm soát khi phù hợp.

## 6. Mẫu sổ theo dõi

| Vòng | SHA đầu vào | Chức năng + lỗi + hiệu năng + code sạch + an toàn + hồi quy | Commit sửa | Test và kết quả | Tồn đọng | Kết luận |
|---|---|---|---|---|---|---|
| 1 | (điền sau) | (phải đủ sáu mục) | (nếu có) | (đúng thực tế) | (ghi rõ) | Chưa kết luận |
| 2 | Chưa bắt đầu | Chưa bắt đầu | — | — | — | Chưa kết luận |
| 3 | Chưa bắt đầu | Chưa bắt đầu | — | — | — | Chưa kết luận |

**Cổng cuối:** Không được ghi `Sẵn sàng trình merge` nếu bất kỳ vòng nào chưa review toàn diện, chưa kiểm chứng hoặc có lỗi nghiêm trọng; chỉ chủ dự án có quyền chấp thuận merge.

## 7. Quy tắc tiết kiệm Token và giữ code tối ưu cho AI

- **Không đọc tràn lan:** Tra `PROJECT_MAP.md` (mục 1 & 3–6) hoặc dùng grep/search để mở đúng file và chỉ đọc đúng khoảng dòng (`StartLine`–`EndLine`) liên quan tới nhiệm vụ. Với `CHANGELOG.md`, chỉ đọc ~50 dòng đầu khi cập nhật phiên bản mới.
- **Không tạo file rác làm phình ngữ cảnh:** Cấm tự tạo thêm các file tóm tắt/báo cáo ngoài luồng (như `AI_CONTEXT.md`, `docs/REVIEW_*.md`). Mọi thay đổi kiến trúc cập nhật vào `PROJECT_MAP.md`, lịch sử cập nhật vào `CHANGELOG.md`.
- **Viết code gọn, tuyến tính, ít nhảy file:** Giữ logic nghiệp vụ liền mạch tại nơi xử lý, dùng cấu trúc dữ liệu tối ưu (`Map`/`Set` `O(1)`, tiền tính `O(n)`, stream I/O) và chỉ comment ngắn gọn những ràng buộc không hiển nhiên để mỗi lần AI đọc lại tốn ít token nhất mà vẫn hiểu trọn vẹn luồng.

## 8. Báo cáo xong việc và thảo luận: GitHub đầy đủ, Telegram tóm tắt song song

- **GitHub là nơi làm việc chính:** mọi ý kiến, phản biện, kết quả review, quyết định đều đăng đầy đủ lên GitHub (issue/PR/file) — đó là bản đầy đủ, có thể kiểm chứng. Telegram không thay GitHub.
- **Xong việc → báo cáo cho chủ dự án, gửi cùng lúc 2 nơi:** 1 tin nhắn báo cáo trong chat và 1 tin tóm tắt tương tự lên Telegram — cùng lúc, cùng nội dung ngắn gọn. Chủ dự án đọc ở đâu cũng nắm được việc.
- **Đọc issue/thảo luận rồi phản hồi → GitHub + Telegram song song:** trả lời đầy đủ cho các AI khác trên GitHub, đồng thời gửi bản tóm tắt lên Telegram để chủ dự án biết — đại khái là báo cáo cho chủ dự án biết AI vừa làm gì, kết luận gì. Không để lệch giờ, không dồn sang hôm sau.
- **Tóm tắt Telegram chỉ ngắn, đặc:** việc gì, nhánh/PR + SHA, kết quả (số lỗi chặn/nên sửa/nhỏ hoặc trạng thái Đủ điều kiện/Chưa đạt), thời gian chạy, kèm link tới comment/PR trên GitHub để đọc bản đầy đủ. Không dán code dài, log dài; không nhắc lại báo cáo cũ.
- **Tin gửi cho các AI khác trên GitHub → báo thêm lên Slack:** những tin nhắn/thảo luận gửi lên GitHub cho các AI khác, ngoài tóm tắt Telegram thì gửi thêm 1 bản tóm tắt ngắn tương tự vào kênh Slack #nouveau-canal — cùng lúc, cùng nội dung ngắn gọn.
- **Merge vào main → báo ngay:** mỗi lần merge vào main phải báo ngay cho chủ dự án trong chat và Telegram.
- **AI nào không tự gửi được Telegram** (chưa có bot/kết nối) thì đăng đầy đủ lên GitHub và nhờ Muse chuyển tiếp tóm tắt sang Telegram — không được bỏ qua bước báo cáo.
- **Không đăng secret, token, API key, mật khẩu lên Telegram hay GitHub.**
- **Tin Telegram chỉ để chủ dự án đọc trên điện thoại, không đánh thức được AI đang đóng.** AI khác đọc thảo luận khi được gọi dậy hoặc theo lịch kiểm tra của mình.
- **Gộp tin, không spam:** nhiều việc xong cùng lúc thì gộp thành 1 tin; không gửi tin cho từng bước nhỏ.

### Mẫu tin nhắn chung gửi lên Telegram (mẫu mềm, không gò bó)

**Quy định:** Mẫu dưới đây là ví dụ chung dành cho ChatGPT, Muse, Jules và mọi AI. Giữ đúng tên AI thực sự tạo nội dung và báo cáo trung thực việc đã làm, kết quả, nguồn GitHub. **AI được tự do thay đổi cách trình bày, emoji, tiêu đề, xuống dòng, thêm/bớt mục hoặc dùng mẫu đẹp hơn** để phù hợp từng loại báo cáo. Không bắt buộc viết giống hệt mẫu hay đủ tất cả nhãn mục khi không cần thiết.

**Mẫu đầy đủ theo nội dung Owner cung cấp (giữ nguyên format và toàn bộ nội dung tin):**

```text
🤖 ChatGPT GPT-6 — BÁO CÁO
Dự án: AI-Collaboration-Hub
Loại thông báo: 🧪 Kiểm tra kết nối Telegram
Nội dung:
ChatGPT đã đọc quy định làm việc chung agents-sub.md và đang phối hợp với Muse, Jules qua GitHub và Slack.
Kết quả:
✅ Đã gửi yêu cầu thử nghiệm cho Muse trên GitHub.
⏳ Chưa xác nhận Muse đã chuyển tiếp thành công lên Telegram.
Nguồn GitHub:
https://github.com/dvphuong20786/AI-Collaboration-Hub/issues/1#issuecomment-6743842530
```

**Lưu ý khi áp dụng:**
- Thay `ChatGPT GPT-6` bằng đúng AI viết báo cáo; thay dự án, loại tin, nội dung, kết quả và link cho đúng sự việc thực tế, không sao chép nguyên báo cáo thử nghiệm như một kết quả mới.
- Nếu AI khác gửi hộ qua bot Telegram chung, ghi rõ `Nội dung của: ChatGPT GPT-6 · Chuyển tiếp bởi: Muse` (hoặc tên AI tương ứng), để không nhầm người tạo với người chuyển tiếp. Có thể thêm dòng này ở bất kỳ vị trí phù hợp.
- Ưu tiên **ngắn, dễ đọc, đủ ý**, gộp tin liên quan để tránh spam. Có PR/nhánh/SHA hoặc thời gian cần báo thì bổ sung; phần phân tích chi tiết giữ ở GitHub.
- Không khẳng định đã hoàn thành khi chưa xác minh; không gửi token, API key, webhook URL hay mật khẩu lên GitHub, Telegram hoặc Slack.

### Mẫu báo cáo mới (khuyến nghị từ 2026-11-10): ngắn, có icon riêng, mỗi mục 1 dòng

Mỗi AI dùng **icon riêng của mình** ở đầu tin (Muse: 🎭, ChatGPT: 🤖; AI mới tự chọn 1 icon riêng, không trùng AI khác). Tin Telegram và tin báo cáo trong chat dùng **cùng một mẫu** để chủ dự án đọc ở đâu cũng quen:

```text
🎭 Muse — BÁO CÁO
Dự án: <tên dự án>
Loại thông báo: ✅ Merge / 🔍 Review / 📣 Quy tắc / 🐛 Lỗi / ...
Nội dung:
<việc gì, 1-2 dòng>
Kết quả:
✅ <đã xong>
⏳ <đang chạy>
⚠️ <cần Owner quyết — mỗi việc 1 dòng>
Nguồn GitHub:
<link tới comment/PR, nếu có>
```

Khi báo cáo lỗi thì thêm 2 dòng tóm tắt (không lan man):
- Lý do lỗi: <tóm tắt 1 dòng>
- Cách giải quyết: <tóm tắt 1 dòng>
- Đề xuất cải tiến (nếu có): <giải pháp tốt hơn / công nghệ tốt hơn / phản biện tốt hơn — tóm tắt 1 dòng; chỉ ghi khi thật sự có ý hay hơn cách hiện tại>

Mẫu này áp dụng cho mọi loại lỗi, kể cả xung đột giữa 2 code/nhánh (conflict).
