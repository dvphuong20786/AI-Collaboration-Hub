# Quy định bắt buộc cho các AI phụ trách nhánh (agents-sub)

> Áp dụng cho mọi AI tham gia phát triển dự án. Đây là quy tắc phân quyền và cổng kiểm duyệt, không phải giấy phép mở rộng phạm vi. Quy tắc từ chủ dự án có ưu tiên cao nhất.

## 1. Một AI – một nhiệm vụ, một nhánh được giao

- Trước khi làm việc, phải biết **AI này được chủ dự án giao PR/nhánh nào**, phạm vi chức năng và những đường dẫn được phép sửa. Không đoán quyền hạn từ nội dung cuộc trò chuyện hoặc một PR đang mở.
- **Cấm sửa/commit/push lên nhánh của AI khác đang thực hiện task.** **Chỉ sau khi AI thực hiện đã push code và báo hoàn thành review toàn diện vòng 1/3 trên GitHub**, Leader và AI reviewer được giao mới bắt đầu đọc code/diff của nhánh đó để review, tìm lỗi và phản biện trên Issue/PR. **Quyền review chỉ là quyền đọc/nhận xét, không phải quyền sửa nhánh.** Không tự đọc nhánh ngoài phạm vi review được giao.
- Chỉ sửa phần code **do chính AI đó phụ trách thực hiện trong nhiệm vụ được giao**. File chung có code của nhiều nhánh không mặc nhiên thuộc quyền sở hữu của một AI.
- Khi được giao review nhánh của AI khác, chỉ đọc và báo lỗi/phản biện bằng bằng chứng trên Issue/PR, **không sửa hộ**, cherry-pick hoặc tự merge/rebase. Nếu không được giao quyền review/giải quyết xung đột, báo Leader/Owner để phân công, không tự ý can thiệp.
- Bản sửa lỗi phải ở **đúng nhánh của AI được giao**, hoặc một nhánh fix được chủ dự án cho phép rõ ràng và liên kết với nhiệm vụ gốc. Không tự tạo PR fix của AI khác.
- Không tự thay đổi branch protection, workflows, quyền truy cập hoặc bật tác vụ CI/build nặng.

## 2. Ít nhất BA VÒNG review TOÀN DIỆN, tuần tự

**Một vòng review toàn diện = một lượt đọc và đối chiếu mã nguồn trên toàn bộ phạm vi task ở TẤT CẢ sáu khía cạnh dưới đây.** Nếu thông báo cũ dùng từ "PASS vòng 1/2/3" thì chỉ hiểu là **hoàn thành review toàn diện vòng 1/3, 2/3, 3/3**; không phải chạy kiểm thử. Đây là REVIEW CODE, không phải build/test/runtime. Không được đếm ba chủ đề của một lượt đọc code thành ba vòng. Phát hiện lỗi thực sự cần sửa ở bất kỳ vòng nào → sửa và bắt đầu lại từ vòng review toàn diện 1/3.

Mỗi vòng bắt buộc xem xét đủ:
1. **Đúng chức năng/nghiệp vụ và UX:** happy path, thao tác người dùng, tương thích hợp đồng/API, các tình huống biên.
2. **Lỗi/độ ổn định:** luồng bất đồng bộ, race condition, hủy tác vụ, timeout, lỗi hệ thống, làm mất hoặc ghi đè dữ liệu.
3. **Hiệu năng/tài nguyên:** CPU, RAM, render, I/O, cache, giới hạn đồng thời, file/tác vụ/video dài, rò rỉ tài nguyên; triệt tiêu các vòng lặp/tra cứu `O(n²)` không cần thiết, không re-render/reset scroll/vẽ lại Canvas vô ích, không nạp toàn bộ media lớn vào RAM.
4. **Code sạch/khả năng bảo trì & tối ưu cho AI đọc:** đúng phạm vi, tên rõ nghĩa, không lặp thừa, không dead code, không tạo file rác/báo cáo nháp ngoài quy chuẩn, type an toàn, không tạo abstraction/wrapper vô ích làm phân mảnh luồng khiến AI tốn thêm token khi đọc nhiều file, phù hợp AGENTS.md.
5. **Bảo mật/đóng gói/vận hành:** đường dẫn, input, command, secrets, quyền truy cập, tương thích hệ điều hành và tài nguyên phát hành (nếu liên quan).
6. **Review hồi quy bằng mã nguồn:** đọc code/diff để đối chiếu lỗi đã sửa, các đường chạy chính và nguy cơ làm hỏng chức năng cũ; chưa chạy build/test thực tế ở giai đoạn này.

Thứ tự tối thiểu:
- **Review toàn diện vòng 1/3:** đọc/đối chiếu toàn bộ code task ở đủ 6 khía cạnh. Không có lỗi phải sửa → ghi nhận vòng 1/3, push nhánh/PR và **báo Leader ngay** để reviewer đọc được thay đổi.
- **Review toàn diện vòng 2/3:** đọc/đối chiếu lại toàn bộ code đang xét, chú trọng hồi quy, trên phiên bản sau vòng 1.
- **Review toàn diện vòng 3/3:** độc lập xem lại toàn bộ code trên phiên bản sau vòng 2; đạt khi không còn lỗi phải sửa.
- **Bất kỳ vòng nào phát hiện lỗi thực sự phải sửa → sửa code, đặt lại tiến độ 0/3 và bắt đầu lại từ vòng 1/3.** Góp ý cải tiến không phải lỗi buộc sửa được ghi riêng, không tự coi là FAIL; không tuyên bố hoàn thành chỉ vì đếm đủ ba lượt.
- Mỗi vòng phải là lượt kiểm tra riêng sau khi đã áp dụng/kết luận lượt trước. **Không chia một đợt đọc code thành nhiều vòng** để đủ chỉ tiêu.
- Nếu không phát hiện lỗi ở một vòng, ghi rõ phạm vi đã xem, cách kiểm chứng và lý do không cần commit sửa code; không tạo commit giả.
- **Giai đoạn này chỉ review code, không yêu cầu build/test/runtime.** Build/test dành cho giai đoạn hoàn thiện/đóng gói theo quyết định Owner. Nếu đề cập build/test trong báo cáo phải ghi đúng **chưa chạy**; không được gọi ba vòng review là ba lần kiểm thử runtime.

## 3. Nhật ký review có thể kiểm chứng

Sau mỗi vòng, báo cáo tối thiểu:
- PR/nhánh và SHA được review; danh sách file **thuộc phạm vi AI**.
- Kết quả cho đủ sáu khía cạnh trên; mức độ ảnh hưởng và file/dòng cho lỗi phát hiện.
- Những sửa đổi đã push, commit SHA; bằng chứng của vòng review toàn diện (code, file/dòng, tình huống và lập luận). Build/test ở giai đoạn này ghi **chưa chạy** nếu có đề cập.
- Rủi ro còn lại, việc thuộc AI khác đã **chỉ báo cáo, không sửa**.
- Trạng thái: `Đang review`, `Chưa đạt`, hoặc `Đủ điều kiện trình chủ dự án`; không tự gắn trạng thái được merge.

## 4. Chốt quyền merge

- **Không push trực tiếp vào `main`; không merge PR, squash, rebase vào `main` hay đóng PR thay người dùng chỉ vì đã hoàn thành.**
- Kết thúc ít nhất ba vòng review toàn diện **không đồng nghĩa** được phép merge.
- Chỉ merge `main` sau khi chủ dự án **xác nhận rõ ràng** PR cụ thể được merge, sau khi đọc báo cáo review/test và các vấn đề tồn.
- AI khác báo hoàn thành → **Leader chịu trách nhiệm review đủ 3 vòng toàn diện, vòng cuối không còn lỗi cần sửa**, trước khi trình merge. **AI thực hiện task không phải theo dõi hoặc đếm số vòng review của Leader/AI khác; chỉ cần tiếp nhận và xác minh các lỗi họ báo.** Quyền merge vẫn theo quyết định Owner hoặc ủy quyền merge hợp lệ.
- Nếu PR bị conflict với nhánh của AI khác, không tự hòa giải. Báo chủ dự án để điều phối người có quyền; chưa được merge.
- Các nhánh fix hoặc PR khác không được tính là quyền sở hữu mới chỉ vì AI đó từng mở hoặc sửa chúng.

## 5. Phạm vi AI hiện tại

- **AI đang thực hiện task:** chỉ sửa đúng nhánh/PR do Owner giao hoặc **Leader giao hợp lệ trên GitHub**; không cần Owner nhắn lại trong cuộc trò chuyện. Chỉ thị rút/hủy mới hơn được ưu tiên.
- **Lệnh giao việc của Leader có hiệu lực ủy quyền thực hiện task từ Owner.** Trong dự án video-ai hiện nay **Muse là Leader**; ở dự án khác Leader có thể là AI/người khác. Agent đọc task GitHub, kiểm chứng lỗi có đúng rồi thực hiện ngay hoặc phản biện có bằng chứng; không tự suy ra quyền merge từ quyền giao task.
- **Không sửa nhánh/PR của AI khác.** Chỉ đọc nhánh/PR khác khi được giao vai trò review; không tự nhận lại task đã rút, không tự mở hoặc phục hồi PR ngoài phạm vi.
- PR chưa có phép merge của chủ dự án thì không được tự merge. Xung đột giữa nhánh do AI khác viết là việc cần chủ dự án phân quyền, không tự giải quyết.
- Giai đoạn nhận/sửa task áp dụng review mã nguồn, **không kích hoạt build/test/runtime**; hoạt động build/test phục vụ giai đoạn hoàn thiện/đóng gói theo chỉ đạo Owner.

## 6. Mẫu sổ theo dõi

| Vòng review toàn diện | SHA đã review | Chức năng + lỗi + hiệu năng + code sạch + an toàn + hồi quy | Commit sửa (nếu có) | Bằng chứng review code | Tồn đọng | Kết luận |
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

## 9. Không overthinking

- Mọi AI không overthinking: chỉ kiểm chứng đủ để quyết định, không phân tích thêm vòng mới khi đã đủ bằng chứng; không tranh luận lặp lại cùng một finding; khi đủ thông tin thì làm hoặc trình Owner quyết, không kéo dài.

## 10. Báo cáo nhánh fix khi mở nhánh

- Mỗi lần fix mở nhánh mới thì báo ngay trong chat: sửa mục nào, mở nhánh nào (tên nhánh + head), vì sao mở thêm nhánh (nếu 1 mục mở nhiều nhánh). Không để Owner phải hỏi mới biết.

## 11. Quy tắc sửa trực tiếp, không cần tạo nhánh

- Sửa file quy tắc (QUY-TAC-CHUNG.md, agents-sub.md, các file dùng chung) thì làm trực tiếp, không cần tạo nhánh — ghi thẳng vào file, đồng bộ 2 chiều như thường.

## 12. Quy trình giao task cho AI khác — từ giao đến merge

- **Giao việc:** Leader đăng task trên GitHub, nêu rõ AI nhận việc, mục tiêu/lỗi, nhánh, base `main`, phạm vi. Mỗi AI một task/một nhánh. **Leader là vai trò theo từng dự án; hiện Muse giữ vai trò Leader của video-ai**, không phải tên cố định cho mọi dự án.
- **Nhận task:** Lệnh giao task hợp lệ của Leader trên GitHub được hiểu là Owner đã đồng ý thực hiện, không cần Owner nhắn lại. AI **tự kiểm tra mã nguồn trước khi làm**: nếu nhận định lỗi đúng/phương án phù hợp thì triển khai; nếu lỗi sai hoặc phương án không đạt mục tiêu thì phản biện bằng SHA, file/dòng và lập luận. Lệnh rút/hủy mới hơn có ưu tiên.
- **Tự review toàn diện 3 vòng liên tiếp:** **Không dùng chữ PASS để chỉ các vòng này.** Review toàn diện nghĩa là đọc/đối chiếu code trên toàn phạm vi task, đủ cả 6 khía cạnh ở mục 2; vòng 2 và 3 là **review hồi quy trên code**, không phải build/test. Trong giai đoạn làm task **không build/test/runtime**; những hoạt động đó thuộc giai đoạn hoàn thiện/đóng gói.
  - **Hoàn thành review toàn diện vòng 1/3:** phải push nhánh/PR để Leader và reviewer đọc được, rồi **báo ngay trên GitHub** kèm nhánh, commit SHA, phạm vi, kết quả và bằng chứng review. AI thực hiện **không chờ Leader** mà tiếp tục hai vòng còn lại.
  - **Hoàn thành review toàn diện vòng 2/3 và 3/3:** báo tiếp lên GitHub, kèm SHA và bằng chứng của từng vòng. Không chia một lượt đọc code thành nhiều vòng.
  - **Nếu bất kỳ vòng nào hoặc phản biện bên ngoài phát hiện lỗi thực sự cần sửa:** AI thực hiện kiểm tra và phản biện; nếu lỗi đúng → sửa rồi **reset tiến độ về 0/3**, bắt đầu lại từ review toàn diện vòng 1/3. Nếu nhận định sai → đăng phản chứng; chưa đủ bằng chứng → ghi chưa xác minh. Một góp ý cải tiến không phải lỗi cần sửa không tự gây reset.
- **Review chéo sau vòng 1:** **chỉ sau khi** AI thực hiện **đã push code và báo hoàn thành review toàn diện vòng 1/3 trên GitHub**, **Leader và AI reviewer được giao bắt đầu đọc/review nhánh đó** trong khi AI thực hiện tiếp tục vòng 2/3 và 3/3; các reviewer báo lỗi trên Issue/PR, **không sửa/commit/push lên nhánh do AI khác phụ trách**. AI thực hiện task **chỉ cần tiếp nhận và xử lý báo lỗi**, **không cần theo dõi/đếm ba vòng review của Leader hoặc AI khác**. Quy trình và số vòng review của từng reviewer là trách nhiệm của reviewer/Leader.
- **Cổng merge:** Leader chịu trách nhiệm review và quyết định kỹ thuật theo quyền, xử lý các phát hiện cần sửa. **Giao task không đồng nghĩa có quyền merge**: chỉ merge khi Owner cho phép PR đó hoặc có ủy quyền merge hợp lệ áp dụng cho PR. Không merge nếu còn lỗi đã xác nhận chưa được xử lý.
- **Kết thúc:** **PR merge thành công vào `main` = task DONE ngay.** Việc dọn nhánh, thông báo hoặc kiểm tra hậu merge không giữ task cũ ở trạng thái đang làm. Nếu sau merge có AI hoặc người khác báo lỗi: vẫn xác minh và phản biện; **nếu lỗi đúng, tạo task bugfix MỚI liên kết task/PR cũ**, phân công/ủy quyền hợp lệ rồi bắt đầu lại quy trình review toàn diện 3 vòng; **không mở lại task đã DONE**.
