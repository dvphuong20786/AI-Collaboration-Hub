# Quy ước riêng khi thiết lập repository

> Bổ sung cho `AGENTS.md`. Đọc `AGENTS.md` trước; các quy tắc dưới đây chỉ áp dụng khi khởi tạo repository mới, tạo scaffold mới, hoặc làm việc trong repository chưa có bộ lệnh developer / bản đồ dự án — không phải quy tắc viết code hằng ngày.

### Quy định bắt buộc: tự tạo bộ công cụ terminal cho developer

- Khi khởi tạo repository mới, tạo scaffold mới hoặc làm việc trong repository chưa có bộ lệnh developer rõ ràng, agent phải chủ động tạo bộ công cụ terminal ngay trong lượt thiết lập đầu tiên; người dùng không cần nhắc lại.
- Trước khi tạo, phải đọc stack, package manager, runtime, database, hệ điều hành mục tiêu và các script hiện có. Không sao chép máy móc lệnh từ dự án khác và không ghi lệnh chưa tồn tại hoặc chưa được kiểm tra.
- Nếu repository đã có script tương đương, ưu tiên chuẩn hóa, bổ sung và đánh số; không tạo thêm một đường chạy trùng gây nhầm lẫn.
- Trên Windows, cấu trúc mặc định là đặt trực tiếp ở thư mục gốc repository để developer nhìn thấy và chạy nhanh:

```text
00_CONG_CU_DEV.cmd
01-kiem-tra-moi-truong.cmd
02-cai-dat.cmd
03-chay-dev.cmd
04-test.cmd
05-build.cmd
06-mo-database.cmd              # Chỉ tạo khi dự án có database/công cụ tương ứng
90-lam-sach.cmd
_NOI_BO_hien-thi-dia-chi.cmd
```

- `00_CONG_CU_DEV.cmd` là điểm bắt đầu được khuyến nghị, hiển thị menu tiếng Việt và gọi đúng các file đã đánh số. Không yêu cầu developer nhớ hoặc tự ghép lệnh dài.
- Bước `01` kiểm tra và in phiên bản các dependency bắt buộc; dependency thiếu phải báo lỗi hoặc cảnh báo rõ, không báo sẵn sàng sai.
- Bước `02` cài dependency, tạo môi trường cục bộ, tạo file cấu hình từ file mẫu khi chưa có, generate client/migration và chuẩn bị database theo stack thực tế. Không ghi đè file cấu hình đã tồn tại.
- Bước `03` chạy đầy đủ môi trường development và in rõ các URL/port có thể mở: frontend, backend health, API docs, worker, database studio hoặc service liên quan.
- Bước `04` chạy toàn bộ test phù hợp; bước `05` build/type-check project. Script phải trả exit code khác `0` khi thao tác thật sự thất bại.
- Bước `06` là công cụ tùy chọn cho database. Nếu dự án không có database hoặc không có giao diện quản trị database thì bỏ bước này và ghi rõ trong tài liệu.
- Bước clean phải dùng số `90` để thể hiện đây không phải thao tác thường xuyên. Trước khi xóa phải yêu cầu người dùng nhập chính xác một từ xác nhận như `XOA`; giá trị khác phải hủy và không thay đổi filesystem.
- Script clean chỉ được xóa danh sách đường dẫn build/cache cụ thể nằm trong repository. Không quét đệ quy qua `node_modules`, dependency, thư mục cha, home directory hoặc ổ đĩa. Không xóa source, `.env`, database, media người dùng, `.venv` hay dependency nếu người dùng chưa yêu cầu rõ.
- Script hỗ trợ không dành cho người dùng chạy trực tiếp phải có tiền tố `_NOI_BO_` và không được mô tả như một bước độc lập.
- Tên file script, biến batch/shell, thông báo terminal và comment do dự án tự đặt phải tuân theo quy tắc tiếng Việt của file này.
- Tạo tài liệu `HUONG_DAN_TERMINAL.md` gồm: thứ tự chạy, yêu cầu môi trường, lệnh cài đặt/dev/test/build/clean, URL local, cách dừng tiến trình và lỗi thường gặp.
- Thêm liên kết từ `README.md` tới `HUONG_DAN_TERMINAL.md` và chỉ rõ lệnh mở menu tổng.
- Với Linux/macOS hoặc dự án đa nền tảng, tạo script tương đương theo shell phù hợp (`.sh`, Makefile hoặc task runner), nhưng vẫn giữ số thứ tự và cùng ý nghĩa các bước nếu công cụ cho phép.
- Sau khi tạo hoặc sửa bộ script, phải chạy tối thiểu: kiểm tra môi trường, test, build, clean ở nhánh hủy và clean thật rồi build lại — nhưng mặc định chạy clean thật trong môi trường/sandbox có fixtures và dry-run; chỉ chạy clean thật trên workspace có dữ liệu người dùng khi được Owner cho phép rõ ràng. Nếu có bước dài hoặc cần credential không thể chạy, ghi rõ phần chưa kiểm thử trong changelog/bàn giao.
- Khi in URL trên terminal, chỉ in địa chỉ đúng với cấu hình thực tế. Không tự mở trình duyệt hoặc dịch vụ bên ngoài nếu người dùng chưa yêu cầu; Electron/desktop app có thể tự mở nếu đó là hành vi của lệnh dev hiện có.

### Quy định bắt buộc: tự tạo và duy trì bản đồ toàn dự án

- Khi khởi tạo repository mới hoặc repository chưa có tài liệu cấu trúc, agent phải tự tạo `PROJECT_MAP.md` ở thư mục gốc; người dùng không cần nhắc lại.
- Project map phải phản ánh code đang tồn tại, không sao chép sơ đồ mẫu hoặc mô tả tính năng chưa triển khai như thể đã hoạt động.
- Nội dung tối thiểu gồm: điểm bắt đầu cho developer, kiến trúc tổng thể, cây thư mục quan trọng, vai trò file/module, API routes, database models, luồng dữ liệu chính, kiểu dữ liệu xuyên tầng, cấu hình/cổng, script terminal, kiểm thử, trạng thái hoạt động/mô phỏng/chưa làm và giới hạn hiện tại.
- Với repository không có API, database, UI hoặc worker, bỏ mục không áp dụng và ghi ngắn gọn lý do; không tạo nội dung giả để đủ mẫu.
- Không liệt kê `node_modules`, `.venv`, build output, cache, secret, `.env` thật, database runtime hoặc mọi asset nhỏ không giúp tìm code.
- Mỗi khi thêm/xóa/đổi tên module quan trọng, route, model database, entry point, biến môi trường, cổng, script developer hoặc luồng xuyên nhiều tầng, phải cập nhật `PROJECT_MAP.md` trong cùng lượt thay đổi.
- Mỗi tính năng phải được phân loại trung thực: hoạt động thật, mô phỏng có nhãn, chưa kiểm thử hoặc chưa triển khai.
- Tên file, module và route trong map phải khớp chính xác repository; trước khi bàn giao phải dùng công cụ tìm file/search để kiểm tra các đường dẫn được ghi trong map còn tồn tại.
- `README.md` phải có liên kết tới `PROJECT_MAP.md` để developer mới tìm thấy ngay.
- Project map dùng tiếng Việt có dấu, sơ đồ text dễ đọc và ưu tiên giúp người đọc trả lời nhanh: “muốn sửa chức năng này thì mở file nào, request đi qua đâu, dữ liệu lưu ở đâu?”.
