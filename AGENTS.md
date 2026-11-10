

# Quy ước chung cho mọi dự án

## Thông tin tác giả mặc định

- Tác giả/phụ trách sản phẩm: `phuongdv`.
- Vai trò: làm PM các ứng dụng và công cụ (Apps Tools).
- Điện thoại liên hệ: `0855550200`.
- Khi tạo phần giới thiệu, màn hình thông tin hoặc metadata cho phần mềm, phải giữ đúng thông tin tác giả này, trừ khi người dùng yêu cầu thay đổi.

# Quy ước viết code cho mọi dự án

File này lưu các yêu cầu chung cho mọi dự án. Khi sửa hoặc tạo mới code, cần ưu tiên các quy ước bên dưới.

## Cách tái sử dụng quy ước giữa các dự án

- File này là bộ quy ước nền, không phụ thuộc tên dự án, đường dẫn máy cá nhân hoặc repository cụ thể.
- Khi sao chép sang dự án khác, giữ các nguyên tắc chung và bổ sung quy định riêng trong một mục có ghi rõ phạm vi áp dụng.
- Không đưa đường dẫn tuyệt đối của máy cá nhân hoặc yêu cầu đồng bộ với một repository khác vào quy ước dùng chung.
- **File dùng chung giữa các dự án** (`AGENTS.md`, `agents-sub.md`, `dev-setup-agent.md`, `frontend-angular-agent.md`, `.agents/skills/`, `QUY-TAC-CHUNG.md`): bản chính nằm ở **AI-Collaboration-Hub** để dự án nào cũng dùng được; khi sửa một file dùng chung thì **đồng thời sửa ở cả hai nơi** (video-ai và hub) để không lệch nhau.

## Thứ tự ưu tiên khi các quy tắc có vẻ chồng chéo

- Trước hết phải đúng nghiệp vụ, đúng hợp đồng API và không làm thay đổi hành vi đang được yêu cầu.
- Sau đó ưu tiên để luồng dữ liệu nhìn thấy trực tiếp từ nơi bắt đầu đến nơi cập nhật giao diện.
- Trong các cách đều rõ luồng, chọn cách đơn giản hơn: ít state, ít hàm trung gian và ít tầng gián tiếp hơn.
- Chỉ tách thêm biến hoặc hàm khi nó làm lộ một bước nghiệp vụ đang khó hiểu; không tách máy móc chỉ để tuân theo ví dụ trong file này.
- Chỉ dùng component/helper chung cho phần kỹ thuật thật sự độc lập. Logic nghiệp vụ cần được giữ gần màn hình sử dụng để dễ đọc và sửa tay.
- Quy tắc chung áp dụng toàn dự án. Quy tắc riêng cho Angular nằm trong `frontend-angular-agent.md`. Phần có tiêu đề nói rõ `báo cáo`, `In/Tải xuống` hoặc một chức năng cụ thể chỉ bổ sung cho đúng phạm vi đó.
- Ví dụ tên biến và tên hàm chỉ minh họa. Tên thực tế phải theo nghiệp vụ, kiểu dữ liệu và giai đoạn xử lý trong file đang sửa.

- Think Before Coding — nói giả định trước, hỏi khi mơ hồ, đừng pick silently
- Simplicity First — tối thiểu code giải quyết vấn đề, tìm đúng nguồn gốc vấn đề thì sẽ code ít nhất, không speculative, không abstraction cho single-use
- Surgical Changes — chỉ chạm cái cần thiết, không "improve" code xung quanh khi chưa được cho phép, match existing style
- Goal-Driven Execution — định nghĩa success criteria đo được, loop đến khi verify xong

## Quy ước riêng cho Angular

- Các quy tắc chỉ áp dụng cho frontend Angular nằm trong `frontend-angular-agent.md` (FormGroup, Input/Output và thẻ custom, change detection, RxJS `finalize()`, template `@if`/`@for`/`@switch`, `@Input`/`@ViewChild`/`ngOnChanges`). Đọc file này trước; chỉ mở file đó khi làm việc với code Angular.

## Mục tiêu chính

- Code phải thông thoáng, tuyến tính và dễ lần theo luồng xử lý.
- Ưu tiên khả năng đọc và bảo trì hơn việc viết ngắn hoặc dùng kỹ thuật quá trừu tượng.
- Người đọc phải dễ tìm được: sự kiện bắt đầu ở đâu, API gọi ở đâu, dữ liệu được map ở đâu và giao diện cập nhật ở đâu.
- Không tự ý tạo thêm tầng kế thừa, base class, helper, mapper hoặc file `*.types.ts` nếu chưa thật sự cần thiết.
- Code tường minh được ưu tiên cao hơn code tổng quát, ngắn hoặc tránh lặp. Nếu abstraction làm người đọc phải nhảy qua nhiều hàm/type/map để hiểu một nghiệp vụ thì phải viết lại đơn giản hơn.
- Chấp nhận lặp một số câu lệnh rõ ràng giữa các nghiệp vụ nếu nhờ đó mỗi chức năng có thể được đọc độc lập từ đầu đến cuối.
- Sau mỗi lần người dùng phản hồi code khó đọc, không chỉ thêm comment; phải xem lại cấu trúc và loại bỏ tầng gián tiếp gây khó hiểu.
## Phạm vi áp dụng và cách đặt tên

### Quy định bắt buộc: ưu tiên tiếng Việt cho mọi tên do dự án tự đặt

- Tất cả vị trí do dự án chủ động đặt tên phải ưu tiên tiếng Việt: biến, hằng số, tham số, hàm, method, class, interface, type, enum, component, service, repository, use case, store, hook, event, field nội bộ, test case, tên file/folder và CSS class.
- Identifier trong code mặc định viết bằng **tiếng Việt không dấu** để tương thích ổn định với compiler, terminal, hệ điều hành, công cụ tìm kiếm và các thư viện. Ví dụ: `duAnDangChon`, `danhSachPhuDe`, `isDangXuLy`, `taiThongTinVideo()`, `taoMatNaVanBan()`.
- Chỉ dùng identifier tiếng Việt có dấu khi ngôn ngữ và toàn bộ toolchain hỗ trợ chắc chắn, dự án đã thống nhất cách dùng và việc đó không làm giảm khả năng tìm kiếm, build, chạy test hoặc làm việc đa nền tảng. Comment, nội dung giao diện, thông báo lỗi và tài liệu phải viết tiếng Việt có dấu đầy đủ.
- Tên phải nói rõ dữ liệu chứa gì, đang ở giai đoạn nào hoặc hàm thực hiện công việc gì. Không chỉ dịch từ tiếng Anh sang một từ tiếng Việt chung chung như `duLieu`, `xuLy`, `thucHien`, `ketQua` nếu có thể đặt tên nghiệp vụ rõ hơn.
- Hàm phải bắt đầu bằng động từ tiếng Việt và nêu rõ đối tượng/tác dụng, ví dụ `docThongTinVideo()`, `kiemTraVungPhuDe()`, `taoMatNaXoaChu()`, `luuDoanPhuDe()`, `capNhatDanhSachHienThi()`.
- Boolean vẫn phải thể hiện rõ trạng thái hoặc khả năng. Ưu tiên các tiền tố tiếng Việt như `isDang...`, `has...` chỉ khi đó là convention framework hiện có; trong code mới có thể dùng `dangXuLy`, `daCoDuLieu`, `coTheXuatVideo`, `canKiemTraLai` nếu đọc rõ nghĩa hơn.
- Tên sự kiện phải thể hiện nguồn thao tác và kết quả mong đợi, ví dụ `khiNguoiDungChonVideo()`, `khiBamNutChayOcr()`, `khiThayDoiVungPhuDe()`; không dùng tên đứng một mình như `xuLy()`, `bam()`, `hanhDong()`.
- Với mảng hoặc collection, tên phải thể hiện số nhiều và nội dung, ví dụ `cacDoanPhuDe`, `danhSachGiongDoc`, `cacKhungHinhCanKiemTra`; không dùng `items`, `list`, `data` nếu có tên tiếng Việt rõ hơn.
- Tên file/folder mới ưu tiên tiếng Việt không dấu khi hệ sinh thái dự án cho phép. Nếu framework, generator, hệ điều hành đóng gói hoặc convention cộng đồng yêu cầu tên tiếng Anh/kebab-case cố định thì giữ convention kỹ thuật đó và dùng tên tiếng Việt rõ nghĩa bên trong code.
- Không tự dịch hoặc đổi tên những phần thuộc hợp đồng bên ngoài: field API đã công bố, cột database đang dùng, biến môi trường, HTTP header, route public, tên package, method framework, lifecycle hook, callback signature, interface thư viện, protocol và định dạng chuẩn. Nếu cần tên tiếng Việt nội bộ, mapping phải viết tường minh tại biên hệ thống.
- Khi override, implement interface hoặc gọi thư viện, giữ nguyên tên bắt buộc của framework/thư viện. Không tạo alias tiếng Việt chỉ để che tên chuẩn nếu alias làm luồng khó lần theo hơn.
- Khi sửa code cũ trong phạm vi được giao, đổi các tên tiếng Anh do dự án tự đặt sang tiếng Việt nếu có thể làm an toàn và đã kiểm tra toàn bộ nơi sử dụng. Không refactor hàng loạt ngoài phạm vi chỉ để đổi tên.
- Nếu bắt buộc phải giữ một tên tiếng Anh không hiển nhiên, thêm comment tiếng Việt ngắn tại nơi khai báo hoặc mapping để giải thích đó là tên từ framework, thư viện hay hợp đồng bên ngoài.

### Comment tiếng Việt và khả năng đọc luồng

- Comment phải viết bằng tiếng Việt có dấu, rõ ràng và tập trung giải thích **vì sao**, dữ liệu đi từ đâu đến đâu, điều kiện nào làm thay đổi state và kết quả ảnh hưởng phần nào của giao diện/hệ thống.
- Các đoạn nhiều bước, công thức khó, xử lý bất đồng bộ, cache, mapping, gọi tiến trình ngoài, invalidation hoặc thay đổi state gián tiếp phải có comment tiếng Việt tại điểm người đọc dễ mất dấu luồng.
- Comment không được dùng để bù cho tên biến/hàm mơ hồ. Trước hết phải đổi tên cho rõ, sau đó mới comment phần lý do hoặc ràng buộc mà tên không thể hiện hết.
- Không comment lặp lại từng câu lệnh đã tự đọc hiểu. Mục tiêu là người mới vào dự án có thể lần theo luồng mà không phải suy đoán, không phải làm file dài một cách máy móc.

- Các quy tắc về code tường minh, đặt tên, state, comment và luồng dữ liệu áp dụng cho toàn bộ dự án: component, service, API, form, modal, table, tab, upload, bản đồ, timeline và các chức năng khác; không chỉ áp dụng cho báo cáo.
- Các tên xuất hiện trong ví dụ như `cacDongDaAnhXa`, `cacDongDangHienThi`, `tongSoChuyen` hoặc `timKiemBaoCao()` chỉ minh họa cách đặt tên. Không sao chép nguyên các tên này sang nghiệp vụ khác.
- Tên phải được đặt theo đúng nghiệp vụ tại file đang sửa. Ví dụ màn tài xế dùng `maTaiXeDangChon`, màn chuyến đi dùng `chiTietChuyenDi`, màn upload dùng `cacAnhGiayPhepDaTaiLen`; không dùng tiền tố của nghiệp vụ khác.
- Tên type/interface dùng danh từ số ít và nói rõ một phần tử là gì, ví dụ `TaiXe`, `ChiTietChuyenDi`, `DongTaiChinh`. Tên biến mảng phải cho biết nó chứa nhiều phần tử và đang ở giai đoạn nào, ví dụ `cacTaiXe`, `cacChuyenDiDaLoc`, `cacDongDangHienThi`.
- Tên boolean phải nói rõ trạng thái, sự tồn tại, khả năng hoặc yêu cầu, ví dụ `dangTaiThongTinTaiXe`, `daCoThongTinGiaoHang`, `coTheCapNhatCauHinhXe`, `canKiemTraLai`.
- Tên hàm bắt đầu bằng động từ tiếng Việt và nói rõ hành động cùng đối tượng, ví dụ `taiChiTietTaiXe()`, `locCacChuyenDi()`, `moHopThoaiXe()`, `taoYeuCauLuu()`.
- Tên CSS class không dùng dấu gạch kép liên tiếp (`__` hoặc `--`); ghép các từ bằng một dấu gạch nối, ví dụ `app-filter-combobox-control-toolbar`.
- Tên sự kiện phải thể hiện nguồn thao tác, ví dụ `khiThayDoiTrangThai()`, `khiBamTimKiem()`, `khiChonTaiXe()`; không dùng `xuLy()`, `thucHien()` hoặc `hanhDong()` đứng một mình.
- Tên dữ liệu phải thể hiện đúng giai đoạn khi việc phân biệt là cần thiết: dữ liệu API gốc, dữ liệu đang nhập, dữ liệu đã áp dụng, dữ liệu sau xử lý và dữ liệu đang hiển thị. Logic đơn giản không được tạo thêm biến chỉ để đủ các giai đoạn theo ví dụ.
- Hậu tố `Maps` chỉ dùng khi biến thực sự chứa dữ liệu đã được map hoặc cấu hình mapping. Không bắt buộc mọi mảng, mọi row hoặc mọi dữ liệu hiển thị đều phải có `Maps`.
- Field API tiếp tục giữ đúng tên API khi ý nghĩa không đổi. Quy tắc đặt tên rõ nghĩa không cho phép tự đổi field API rồi tạo thêm mapping không cần thiết.
- Khi tên ngắn vẫn truyền đạt đầy đủ trong phạm vi component thì ưu tiên tên ngắn, ví dụ `FinancialRow`, `tableRows`. Không ghép quá nhiều từ khiến tên dài nhưng vẫn không làm rõ thêm ý nghĩa.

## Nguyên tắc ưu tiên cao nhất: code phải tường minh

- Code phải thật dễ đọc, dễ theo dõi luồng dữ liệu, dễ tìm chỗ sửa và dễ mở rộng về sau.
- Khi phải lựa chọn, ưu tiên code tường minh hơn code ngắn, code “thông minh” hoặc code tái sử dụng quá mức.
- Người đọc chỉ cần mở folder của một chức năng hoặc một báo cáo là phải thấy được đầy đủ: dữ liệu vào, thao tác người dùng, API, mapping, trạng thái hiển thị, In và Tải xuống.
- Không buộc người đọc phải nhảy qua nhiều component chung, base class, helper, mapper hoặc file type chỉ để hiểu một luồng nghiệp vụ đơn giản.
- Chấp nhận lặp code giữa các nghiệp vụ nếu việc lặp đó giúp từng nghiệp vụ độc lập, dễ đọc và dễ sửa.
- Tên biến và tên hàm phải mô tả đúng nghiệp vụ; tránh các tên chung chung như `data`, `item`, `handle`, `process`, `action` nếu có thể đặt tên rõ hơn.
- Ưu tiên cao nhất là giữ đúng tên field API trả về trong interface, row hiển thị, cấu hình cột và binding HTML.
- Không tự đổi tên field API sang tên ngắn hơn hoặc tên khác ý nghĩa vì sẽ khiến người đọc phải tra mapping.
- Chỉ được đổi tên khi field giao diện mang ý nghĩa thật sự khác field API; đoạn mapping phải viết rõ từng field và có comment tiếng Việt giải thích lý do đổi tên.
- Hàm chuyển một phần tử API sang row được phép và ưu tiên đặt tên có chữ `map`, ví dụ `mapApiRow()` hoặc tên cụ thể hơn theo nghiệp vụ.
- Chỉ biến chứa kết quả thực sự vừa đi qua bước mapping hoặc cấu hình mapping mới dùng hậu tố `Maps`, ví dụ `driverRowMaps`, `exportColumnMaps`.
- Mảng chỉ được lọc, sắp xếp hoặc đưa lên table không tự động mang hậu tố `Maps`; ưu tiên `filteredRows`, `sortedRows`, `tableRows` nếu không có bước mapping.
- Không dùng `Maps` cho state thao tác của người dùng như filter checkbox, giá trị đang chọn, text tìm kiếm. Các biến này phải đặt theo vai trò thật, ví dụ `filterOptions`, `selectedFilterOptions`, `draftSelectedFilterOptions`.
- Với state có hai bước, phải phân biệt rõ trong tên biến:
  - `draft...` là giá trị người dùng đang sửa trong popup/form, chưa ảnh hưởng dữ liệu chính.
  - `selected...` hoặc `applied...` là giá trị đã được xác nhận và đang áp dụng cho table, API hoặc giao diện chính.
- Không dùng hậu tố số, tên chung chung hoặc tên gần giống nhau cho hai state khác vai trò. Đọc tên biến phải biết giá trị đang tạm chọn hay đã áp dụng.
- Khi map mảng, viết rõ `cacDongApi.map(dongApi => this.anhXaDongApi(dongApi))`; phần tử nguồn đặt tên `dongApi`, không dùng tên chung chung như `phanTu` hoặc `duLieu` nếu có tên nghiệp vụ rõ hơn.
- Mỗi sự kiện trên HTML phải lần được trực tiếp tới một hàm có tên rõ nghĩa trong component TypeScript.
- Mỗi API phải có một hàm gọi riêng, dễ tìm; mapping response phải nằm gần hàm gọi API hoặc trong một hàm mang tên nghiệp vụ rõ ràng.
- Comment tiếng Việt tại những đoạn không hiển nhiên, tập trung giải thích luồng dữ liệu và lý do xử lý.

## Đọc một lần phải thấy được luồng

- Code phải được viết cho người bảo trì đọc, không viết để thể hiện kỹ thuật hoặc cố rút ngắn số dòng.
- Một giá trị nghiệp vụ chỉ có một nguồn dữ liệu chính và một kiểu dữ liệu xuyên suốt. Ví dụ filter chọn đơn chỉ là `status` hoặc `null`, không được lúc là `status`, lúc là `[status]`.
- Không viết hàm “chuẩn hóa” để âm thầm hỗ trợ nhiều kiểu dữ liệu do code trước đó tạo ra. Phải sửa ngay nơi gán dữ liệu để toàn bộ luồng dùng cùng một kiểu.
- Khi nghiệp vụ chọn đơn, viết phép so sánh trực tiếp như `selectedStatus === status`; không chuyển thành mảng rồi dùng `includes()`.
- Mỗi thao tác phải đọc được theo thứ tự từ trên xuống: lấy giá trị hiện tại → xác định giá trị mới → lưu giá trị → gọi API hoặc cập nhật giao diện.
- Trong hàm xử lý sự kiện, ưu tiên các biến có tên cụ thể như `selectedStatus`, `newStatus`, `searchParams`; không dùng tên mơ hồ như `value`, `data`, `result` nếu có tên nghiệp vụ rõ hơn.
- Không giữ hai state biểu diễn cùng một ý nghĩa nếu không thật sự cần thiết. Nếu có state cho form và state cho component giao diện, phải có một hàm đồng bộ duy nhất với tên thể hiện rõ mục đích.
- Với logic có nhiều bước hoặc phép tính khó nhìn, chủ động tạo biến trung gian có tên rõ nghĩa để làm lộ từng giai đoạn của luồng dữ liệu.
- Chỉ tạo state trung gian khi HTML hoặc một thao tác về sau thực sự cần dùng lại giá trị đó. Nếu giá trị chỉ dùng tiếp trong cùng một hàm, dùng biến cục bộ để tránh tạo state thừa.
- Với logic quá đơn giản, đọc một dòng đã hiểu và không có giai đoạn nghiệp vụ riêng thì không tạo thêm state. Không sao chép một giá trị sang nhiều biến chỉ để làm code dài hơn.
- Số lượng state phụ thuộc độ khó của luồng: logic khó có thể tách thêm state để dễ theo dõi; logic dễ giữ trực tiếp một state duy nhất.
- Chỉ dùng `draft...` và `applied...` khi cần làm rõ hai giai đoạn đang sửa tạm và đã áp dụng. Nếu giao diện không có hai giai đoạn này thì không tạo đủ bộ tên theo khuôn mẫu.
- Không thêm nhánh tương thích với kiểu dữ liệu cũ sau khi đã thống nhất model mới, trừ khi dữ liệu thực sự đến từ API bên ngoài và chưa thể kiểm soát.
- Không dùng `any` cho dữ liệu nghiệp vụ mới nếu có thể khai báo một interface nhỏ, rõ ngay trong file component.
- Hàm đơn giản không cần comment giải thích từng câu lệnh. Comment chỉ giải thích lý do tồn tại của đoạn xử lý mà tên biến và tên hàm chưa thể hiện được.
- Trước khi hoàn thành, đọc lại code như một người mới vào dự án. Nếu phải suy đoán một biến chứa kiểu gì hoặc phải tìm nhiều nơi mới biết ai thay đổi nó, cần viết lại cho rõ hơn.
- Ưu tiên cấu trúc điều kiện giống câu nói nghiệp vụ. Nếu người dùng “bấm lại trạng thái đang chọn thì bỏ chọn, bấm trạng thái khác thì chọn mới”, code cũng phải có hai nhánh `if` theo đúng thứ tự đó.
- Với điều kiện mang ý nghĩa nghiệp vụ, ưu tiên `if` và `return` rõ ràng hơn toán tử ba ngôi, biểu thức boolean dài hoặc callback lồng nhau.
- Tên tham số sự kiện phải thể hiện nguồn của giá trị, ví dụ `clickedStatus`; tên biến state phải thể hiện vai trò, ví dụ `currentStatus`.
- Hàm sự kiện không nên đồng thời chuẩn hóa kiểu dữ liệu, sửa constant, dựng API params và gọi API. Mỗi bước phức tạp phải có hàm mang tên đúng nghiệp vụ.
- Đối với luồng tìm kiếm, thứ tự mặc định là: đọc state form → kiểm tra hợp lệ → dựng params → bật loading → gọi API → nhận response → map dữ liệu → cập nhật state hiển thị.

## Không ẩn luồng dữ liệu

- Quy tắc này áp dụng cho toàn bộ dự án, không chỉ báo cáo, filter hoặc table.
- Người sửa code phải nhìn thấy trực tiếp: dữ liệu nguồn ở đâu → điều kiện nào được kiểm tra → dữ liệu trung gian sau mỗi bước → state cuối cùng được gán ở đâu → HTML/component nào sử dụng state đó.
- Không dồn nhiều bước lọc, map, sort, chuẩn hóa và gán state vào một biểu thức hoặc một chuỗi callback. Tách từng bước và đặt tên dữ liệu trung gian theo đúng kết quả của bước đó.
- Với nhiều điều kiện lọc độc lập và mỗi điều kiện đọc ngay đã hiểu, ưu tiên một `.filter()` tuyến tính với các câu `if (...) return false`; không tách thành quá nhiều biến boolean làm code dài hơn.
- Chỉ chuyển sang `for...of` và state trung gian chi tiết khi callback thực sự có nhiều bước nghiệp vụ, nhiều nhánh phụ hoặc tác dụng phụ khó theo dõi.
- Không dùng chuỗi `.filter().map().sort().reduce()` cho luồng nghiệp vụ nếu người đọc phải tự suy luận dữ liệu thay đổi thế nào qua từng callback.
- `.map()`, `.filter()` và `.sort()` được dùng trực tiếp khi mỗi callback chỉ có một mục đích và đọc từ trên xuống đã hiểu. Không kéo dài code chỉ để tránh callback tiêu chuẩn.
- Với phép tính khó, tách tên theo đúng giai đoạn như `apiRows`, `mappedRows`, `filteredRows`, `sortedRows`, `tableRows`. Chỉ `mappedRows` là kết quả mapping; không gắn `Maps` máy móc cho các giai đoạn còn lại.
- Khi các field nghiệp vụ đã biết trước, ưu tiên đọc field trực tiếp như `driverRow.totalDrivers`; hạn chế `row[column]`, object động và ép kiểu nếu chúng làm người đọc không biết field nào đang được xử lý.
- Không đặt tên hàm chung chung rồi giấu nhiều tác dụng phụ bên trong. Tên hàm phải cho biết hàm đọc dữ liệu nào, xử lý gì và state nào sẽ thay đổi.
- Một hàm không được âm thầm vừa reset filter, đóng popup, map API và cập nhật table nếu tên hàm không thể hiện đầy đủ các việc đó. Tách thành các bước nhìn thấy được tại nơi gọi.
- Không thay đổi state quan trọng trong getter, setter, pipe, lifecycle hook, toán tử RxJS hoặc helper dùng chung mà nơi kích hoạt không thể hiện rõ. Nếu bắt buộc, phải có hàm sự kiện/tên nghiệp vụ và comment tại điểm kích hoạt.
- Tại nơi gọi hàm xử lý phức tạp, viết comment ngắn gọn, súc tích, không lê thê dài dòng mô tả kết quả đầu ra nếu tên hàm vẫn chưa đủ để thấy ảnh hưởng lên giao diện.
- Khi đọc lại code mà phải nhảy qua nhiều hàm mới biết dữ liệu bị lọc, đổi tên, reset hoặc gán ở đâu, phải viết lại tuyến tính hơn trước khi bàn giao.

## Cấu trúc riêng của từng chức năng

- Mỗi chức năng nghiệp vụ nên là một khối độc lập trong folder của nó, với tên bám theo nghiệp vụ để dễ tìm.
- Logic nghiệp vụ, mapping dữ liệu và state hiển thị phải nằm gần màn hình hoặc use case sử dụng chúng.
- Component/helper dùng chung chỉ chứa xử lý kỹ thuật thực sự độc lập; không giấu quy tắc nghiệp vụ trong abstraction tổng quát.
- Khi danh sách field đã biết trước, ưu tiên đọc field trực tiếp thay vì object động hoặc ép kiểu khiến người đọc phải suy đoán.
- Sau khi thay luồng xử lý, xóa component, hàm, interface và import cũ không còn dùng để tránh tồn tại hai đường xử lý.

## Cách viết hàm

### Quy định không dùng accessor

- Không khai báo accessor bằng cú pháp `get tenThuocTinh()` hoặc `set tenThuocTinh(...)`, kể cả getter đơn giản, `@Input` setter và `@ViewChild` setter như `get shareTable()` / `set shareTable(...)`.
- Dùng thuộc tính thông thường để lưu state và hàm có tên rõ nghĩa được gọi tường minh để xử lý.
- Quy định cấm cú pháp accessor, không cấm hàm thông thường có tên bắt đầu bằng `get` hoặc `set`, ví dụ `getBodyAndParams()` và `setStateUrl()`.
- Không tự refactor các accessor cũ ngoài phạm vi được giao; nếu thay đổi accessor đang thuộc hợp đồng kế thừa thì phải kiểm tra các nơi sử dụng trước.

- Mỗi hàm chỉ nên đảm nhiệm một việc rõ ràng.
- Đặt tên theo nghiệp vụ, ví dụ `loadTripDetails()`, `buildExcelRows()`, `downloadPdf()`.
- Ưu tiên `if` và `return` rõ ràng; hạn chế toán tử ba ngôi lồng nhau.
- Hạn chế callback lồng sâu và chuỗi RxJS phức tạp khi `async/await` hoặc các hàm nhỏ dễ đọc hơn.
- Không giấu logic nghiệp vụ quan trọng trong getter.
- Không viết getter để dựng hoặc biến đổi dữ liệu như `get exportRows()`, `get exportDateRange()` hoặc `get reportPages()`.
- Với dữ liệu thay đổi theo thao tác người dùng, dùng biến trạng thái và hàm cập nhật có tên rõ ràng. Ví dụ: nhập từ khóa → `onFilterSearchTextChanged()` → cập nhật `displayedFilterOptions`.
- Không gọi trực tiếp hàm lọc, map, sort, dựng mảng hoặc tính toán dữ liệu trong template. Template chỉ đọc biến trạng thái đã chuẩn bị sẵn.
- Mỗi thao tác trên template chỉ gọi một hàm sự kiện rõ nghĩa. Hàm sự kiện chịu trách nhiệm cập nhật state theo thứ tự nhìn thấy được trong file TypeScript.
- Ưu tiên để các phần trong component theo thứ tự: Input/Output → state API → state đang hiển thị → state tạm của popup/form → sự kiện người dùng → gọi API → mapping → hàm hiển thị đơn giản.
- Khi một hàm thay đổi nhiều state, nhóm câu lệnh theo đúng luồng nghiệp vụ và dùng dòng trống để tách từng bước; không xen kẽ state tạm, state đã áp dụng và dữ liệu hiển thị.
- Không tạo hàm trung gian chỉ để đổi tên hoặc gọi tiếp đúng một hàm khác. Chỉ tách hàm khi tên hàm mới giúp người đọc hiểu một bước nghiệp vụ độc lập.
- Nếu một biến chỉ được thay đổi bởi một sự kiện, đặt biến gần nhóm state của sự kiện đó và đặt hàm xử lý gần các hàm cùng luồng.
- Tránh tên hàm chung chung như `updateVisibleRows()`, `updateState()` hoặc `processData()` vì chúng che việc hàm đang map, lọc, sắp xếp hay reset dữ liệu.
- Tên hàm phải nói rõ hành động và đối tượng, ví dụ `mapApiRows()`, `resetTableFilters()`, `filterTableRows()` hoặc `sortTableRows()`. Không ghép nhiều hành động vào một tên để hợp thức hóa một hàm đang làm quá nhiều việc.
- Khi một hàm gọi tiếp hàm tính toán khác, comment tại điểm gọi nếu kết quả làm thay đổi dữ liệu ở nơi khác trên giao diện. Người đọc không được phải đoán table hoặc checkbox được cập nhật ở đâu.
- Với chức năng In/Tải xuống, dùng biến thường như `exportRows`, `exportDateRange` và chuẩn bị chúng trong hàm `prepareExportData()` ngay trước khi mở popup.
- Luồng phải đọc được theo thứ tự: người dùng bấm nút → `prepareExportData()` → gán dữ liệu xuất → mở popup.
- Tải PDF/XLSX và In phải dùng toàn bộ dữ liệu phù hợp, không chỉ dữ liệu của trang table đang nhìn thấy.
- “Số dòng mỗi trang” chỉ điều khiển cách chia trang trong file/PDF hoặc bản In; không được dùng để cắt mất dữ liệu xuất. Giá trị mặc định là lựa chọn lớn nhất đang hỗ trợ.
- Không hard-code ngày mẫu trong báo cáo. Ngày bắt đầu/kết thúc mặc định phải lấy từ ngày hiện tại, trừ khi nghiệp vụ quy định khác.
- Nếu chỉ cần tính dữ liệu hiển thị trong component dùng chung, dùng hàm có tên hành động rõ ràng như `buildReportPages()`; không dùng cú pháp getter `get ...` để che việc đang chạy xử lý.
- Không dùng generic nếu kiểu cụ thể giúp code dễ hiểu hơn.
- Không ghép nhiều utility type như `Partial<Record<keyof T, ...>>` cho state nghiệp vụ thông thường. Ưu tiên interface hoặc object có tên key/value rõ ràng để người đọc dễ sửa tay.
- Không dùng `keyof` trong state, tham số hàm, ép kiểu hoặc model của code nghiệp vụ. Nếu danh sách field cố định, khai báo trực tiếp union các tên field như `'period' | 'totalTrips'` để nhìn thấy ngay các giá trị hợp lệ.
- Không dùng `Partial<Record<keyof T, ...>>`, `column as keyof T` hoặc generic tương tự chỉ để truy cập field theo tên cột.
- Không dùng `Reflect.get()` để đọc field nghiệp vụ. Nếu buộc phải đọc field theo tên động, dùng cú pháp thông thường `row[columnName]` và comment ví dụ field cụ thể.
- Tách luồng theo thứ tự: nhận thao tác → kiểm tra dữ liệu → gọi API → map dữ liệu → cập nhật giao diện.
- Xóa hàm, mapper, interface và import không còn sử dụng sau khi chuyển đổi model.

## Component và model

- Component nào sử dụng API và model riêng thì đặt interface gần component đó nếu interface không được dùng chung thật sự.
- Không dùng model danh sách cho API chi tiết và không dùng model chi tiết cho danh sách.
- Component cha chỉ điều phối và truyền tham số; component nghiệp vụ tự quản lý API, mapping và trạng thái của nó.
- Không dồn xử lý API, PDF, Excel, popup và mapping của nhiều báo cáo vào một component lớn.
- Chỉ tách service/helper khi có trách nhiệm độc lập rõ ràng hoặc được sử dụng ở nhiều nơi.

## Comment trong code

- Không comment mọi câu lệnh. Đoạn code tự đọc đã hiểu thì để code tự diễn đạt, tránh comment làm file dài và rối hơn.
- Ưu tiên comment tại nơi luồng chạy bị ẩn hoặc khó lần theo: Input/Output giữa component, lifecycle hook, subscription, state điều khiển `@if`, dữ liệu tạm và dữ liệu đã áp dụng, API được kích hoạt gián tiếp.
- Comment phải làm rõ một thay đổi ở dòng hiện tại khiến dữ liệu hoặc giao diện tại nơi khác thay đổi như thế nào.
- Với đoạn lọc, map hoặc sắp xếp khó hình dung, comment phải có một ví dụ dữ liệu ngắn: dữ liệu đầu vào, điều kiện đang áp dụng và kết quả đầu ra. Không chỉ mô tả chung chung rằng đoạn code đang lọc hoặc sort.
- Comment bằng tiếng Việt tại những đoạn khó hiểu hoặc cần giải thích lý do.
- Comment cần giải thích “tại sao làm như vậy”, không lặp lại nguyên văn câu lệnh.
- Không viết comment dài cho đoạn code vốn đã rõ nhờ tên hàm và tên biến.
- Mặc định chỉ dùng comment một dòng `//`.
- Không dùng block comment `/** ... */` hoặc `/* ... */` trong code nghiệp vụ.
- Nếu nội dung comment cần nhiều dòng, viết thành nhiều dòng `//` liên tiếp để dễ đọc và dễ sửa.
- Chỉ dùng JSDoc `/** ... */` khi một công cụ tạo tài liệu hoặc thư viện public thực sự bắt buộc; không dùng JSDoc cho component, field và hàm nội bộ thông thường.

Ví dụ comment đúng:

```ts
// API trả thời gian UTC nhưng màn hình cần hiển thị theo giờ Việt Nam.
const displayedStartTime = TimeUtils.toVietnamTime(apiStartTime);
```

Không viết:

```ts
/**
 * API trả thời gian UTC nhưng màn hình cần hiển thị theo giờ Việt Nam.
 */
const displayedStartTime = TimeUtils.toVietnamTime(apiStartTime);
```

## Cách làm form mới

- Mỗi form/popup nên có component riêng khi có nghiệp vụ hoặc API riêng.
- File component nên thể hiện rõ bốn phần: dữ liệu đầu vào, trạng thái form, sự kiện người dùng và gọi API.
- Đặt tên field giống tên nghiệp vụ/API; tránh tên viết tắt khó đoán.
- Khởi tạo giá trị form trong một hàm rõ ràng như `initializeForm()` hoặc `loadFormData()`.
- Khi mở form sửa, map dữ liệu API vào form tại một vị trí duy nhất.
- Khi lưu, tạo payload trong hàm riêng như `buildSaveRequest()`; không dựng payload dài trực tiếp trong `subscribe()`.
- Validation phải thể hiện trực tiếp và dễ đọc. Thông báo lỗi đặt gần field tương ứng.
- Trạng thái disable phải có hàm hoặc biến mang tên nghiệp vụ, ví dụ `isBankInformationReadonly`, không dùng biểu thức dài trực tiếp trong HTML.
- Các field phụ thuộc nhau phải xử lý trong hàm có tên rõ ràng. Ví dụ chọn ngân hàng thì gọi `onBankChanged()` và hàm này chịu trách nhiệm xóa số tài khoản, tên tài khoản.
- API tra cứu theo dữ liệu nhập phải debounce và chỉ gọi khi dữ liệu hợp lệ, tránh gọi liên tục.
- Trong lúc lưu phải có biến trạng thái rõ ràng như `isSaving`; chống bấm nút nhiều lần.
- Khi API lỗi, giữ lại dữ liệu người dùng đã nhập và hiển thị thông báo phù hợp.
- Nút đóng, nút hủy và phím `Esc` phải đóng được popup nếu không có yêu cầu đặc biệt ngăn đóng.
- HTML ưu tiên điều kiện đơn giản; đưa biểu thức nghiệp vụ dài về component TypeScript.

## FormGroup và state đơn giản

Xem `frontend-angular-agent.md` (mục FormGroup và state đơn giản).

## Luồng dữ liệu của component và thẻ custom

Xem `frontend-angular-agent.md` (mục Luồng dữ liệu của component và thẻ custom).

## API và trạng thái tải dữ liệu

- Mỗi API chỉ nên có một điểm gọi dễ tìm trong component/service phụ trách.
- Chức năng nào sử dụng dữ liệu thì phải gọi trực tiếp API tương ứng tại nơi phụ trách chức năng đó, để nhìn vào code là thấy ngay API được gọi ở đâu và response cập nhật dữ liệu nào. Không lấy dữ liệu gián tiếp qua một service trung gian, store dùng chung hoặc subscription của luồng khác nếu dữ liệu không thật sự dùng chung.
- Chỉ dùng cơ chế lưu trạng thái dùng chung khi dữ liệu thật sự được nhiều chức năng cùng sử dụng và cần đồng bộ. Không đưa dữ liệu riêng của một chức năng vào state dùng chung chỉ để tránh gọi API trực tiếp.
- Khi một màn cha chứa nhiều báo cáo/component con, mỗi báo cáo phải có hàm tải dữ liệu mang tên nghiệp vụ rõ ràng, ví dụ `loadTripSourceReport()`.
- Mỗi báo cáo phải có biến dữ liệu riêng như `tripSourceRows`; không dùng một biến chung tên `rows` cho nhiều loại báo cáo nếu điều đó làm khó lần luồng.
- Template cha phải truyền đúng biến có tên nghiệp vụ xuống component con, ví dụ `[rows]="tripSourceRows"`, để nhìn HTML là biết dữ liệu đến từ đâu.
- Dù nhiều báo cáo tạm thời dùng chung một endpoint, vẫn phải thể hiện rõ nhánh gọi API và dữ liệu nhận về của từng báo cáo. Chấp nhận lặp một ít code để dễ đọc.
- Không gọi lại API chi tiết khi chuyển tab popup nếu dữ liệu đã tải và không có yêu cầu làm mới.
- Không lấy dữ liệu chi tiết từ item của API danh sách để bù cho API chi tiết.
- Với dữ liệu phân trang cần xuất file, phải tải đủ tất cả trang trước khi tạo PDF/XLSX.
- Tên biến trạng thái phải rõ nghĩa: `isLoadingList`, `isLoadingDetail`, `isSaving`, `loadedDates`.
- Luôn chống gọi API trùng khi thao tác người dùng xảy ra liên tục.

## Luồng thao tác phải hiển nhiên, không xử lý ngầm

- Quy ước này áp dụng cho toàn bộ dự án, không chỉ riêng màn Báo cáo.
- Một thao tác của người dùng phải có điểm bắt đầu rõ ràng trong HTML, ví dụ `(click)="searchReport()"`, và có thể lần thẳng tới hàm TypeScript xử lý thao tác đó.
- Component con chỉ gọi API khi hàm `searchReport()` được gọi trực tiếp. Việc component được tạo lại, mở/đóng popup hoặc thay đổi Input phục vụ hiển thị không được tự động gọi API ngoài ý muốn.
- Trong template cha viết rõ dữ liệu hiển thị truyền xuống và dữ liệu/loading trả lên. Tham số tìm kiếm được truyền trực tiếp khi cha gọi `searchReport()` để lần theo luồng từ nút Lọc tới API.
- Hàm xử lý nút tìm kiếm phải thể hiện tuần tự: kiểm tra điều kiện → chốt tham số → xóa dữ liệu cũ nếu cần → phát tín hiệu tải → bật loading.
- Hàm gọi API trong component con phải thể hiện tuần tự: phát loading `true` → gọi API → map response → cập nhật dữ liệu → phát dữ liệu lên cha → kết thúc loading trong cả trường hợp thành công và lỗi.
- Trạng thái loading phải có đúng một luồng bật và một luồng tắt dễ tìm. Không được bật loading ở một nhánh mà nhánh đó có khả năng không gọi API hoặc không bao giờ tắt loading.
- Khi chuyển tab, đổi báo cáo hoặc đóng form, phải chủ động reset các trạng thái tạm của màn cũ nếu chúng không còn ý nghĩa.
- Tránh dùng các cờ chung chung như `loading`, `action`, `changed`. Ưu tiên tên theo nghiệp vụ như `isLoadingPlatformReport`, `isSavingDriver`.
- Khi sửa một lỗi luồng dữ liệu, phải sửa nguyên nhân và làm rõ lại luồng; không chỉ thêm một lệnh reset hoặc timeout để che triệu chứng.

## Tiêu chí tự kiểm tra độ dễ hiểu

Trước khi bàn giao một chức năng, tự đọc lại code theo các câu hỏi sau:

1. Người dùng bấm ở đâu?
2. Hàm nào nhận thao tác đó?
3. Tham số tìm kiếm được chốt ở đâu?
4. Component nào gọi API và hàm gọi API tên gì?
5. Response được map ở đâu?
6. Dữ liệu được đưa lên HTML hoặc trả về component cha ở đâu?
7. Loading được bật và tắt tại đâu?
8. Khi API lỗi, giao diện và loading trở về trạng thái nào?

Nếu phải tìm qua nhiều file không liên quan hoặc không trả lời nhanh được các câu hỏi trên, cần viết lại luồng cho tường minh hơn trước khi bàn giao.

## HTML và style

- Style dùng ở nhiều màn hình phải đặt trong component dùng chung tương ứng hoặc file style chung, để các màn hình cùng nhận thay đổi từ một nơi; quy tắc này áp dụng cho mọi dự án, không giới hạn ở frontend hoặc báo cáo.
- Không nhồi logic nghiệp vụ dài vào binding HTML.
- Trong template đơn giản, biến lặp có thể là `row`. Khi có nhiều loại dữ liệu hoặc nhiều vòng lặp lồng nhau, đặt tên theo nghiệp vụ như `driverRow`, `passengerRow`; chỉ dùng hậu tố `Maps` nếu phần tử thực sự là kết quả mapping.
- CSS nội bộ của component kỹ thuật trong `src/app/pages/report/component` được giữ cạnh component khi style đó chỉ phục vụ riêng component.
- Style form mới phải bám theo giao diện mới của màn Chuyến đi nếu không có design riêng.
- Chiều cao input, select và button cần đồng nhất theo màn tham chiếu.
- Nội dung có thể tràn cột phải có ellipsis và tooltip hiển thị đầy đủ khi hover.

## Kiểm tra trước khi bàn giao

### Quy định bắt buộc: tự tạo bộ công cụ terminal cho developer

Xem `dev-setup-agent.md` (mục tự tạo bộ công cụ terminal cho developer).

### Quy định bắt buộc: tự tạo và duy trì bản đồ toàn dự án

Xem `dev-setup-agent.md` (mục tự tạo và duy trì bản đồ toàn dự án).

- Chạy kiểm tra TypeScript hoặc build phù hợp sau khi sửa.
- Không để quá trình kiểm tra tự ý thay đổi version; nếu script build tự tăng version thì phải hoàn lại thay đổi phát sinh do kiểm tra.
- Không sửa hoặc rollback những thay đổi khác đang có trong working tree.
- Báo rõ phần đã kiểm tra và các lỗi môi trường không liên quan nếu có.

## Quy tắc viết code để junior dễ đọc

- Viết code theo đúng thứ tự người dùng thao tác: nhận dữ liệu → kiểm tra → xử lý → gọi API → cập nhật giao diện.
- Ưu tiên tên biến, tên hàm và tên class nói rõ nghiệp vụ; tránh tên chung chung như `data`, `item`, `value` hoặc `handle` khi có thể đặt tên cụ thể hơn.
- Không gom nhiều nghiệp vụ vào một hàm hoặc một biểu thức dài. Tách thành các bước nhỏ khi việc tách giúp người mới hiểu luồng dễ hơn.
- Comment bằng tiếng Việt cho phần xử lý không hiển nhiên, tập trung giải thích lý do; không dùng comment để che một cấu trúc code khó đọc.
- Trước khi bàn giao, đọc lại như một junior mới vào dự án: nếu phải đoán kiểu dữ liệu, điểm gọi API hoặc nơi cập nhật giao diện thì phải viết lại cho tường minh hơn.

## Quy tắc bắt buộc về Code sạch, Tối ưu hiệu năng và Tiết kiệm Token cho AI

### 1. Code sạch (Clean Code & Không dư thừa)

- Không để lại dead code, biến/hàm/import không dùng, câu lệnh gán lặp vô nghĩa hoặc hai cơ chế song song làm cùng một việc.
- Không tạo file nháp, báo cáo review riêng lẻ (`docs/REVIEW_*.md`, `AI_CONTEXT.md`), hoặc tài liệu trùng lặp ngoài các file chuẩn của dự án (`README.md`, `PROJECT_MAP.md`, `CHANGELOG.md`, `AGENTS.md`).
- Chỉ chạm đúng vùng code giải quyết yêu cầu; không tự ý "cải thiện" (refactor/format) code xung quanh ngoài phạm vi khi chưa được phép.

### 2. Tối ưu hiệu năng thực chất (Performance First)

- **Độ phức tạp thuật toán**: Tránh vòng lặp lồng nhau `O(n²)` trên danh sách lớn (ví dụ: `.find()` / `.includes()` / `min()` bên trong `.filter()` / `.map()` / vòng lặp); ưu tiên tạo `Map` / `Set` tra cứu `O(1)` hoặc tiền tính một lượt `O(n)`.
- **Giao diện & Render**:
  - Không gắn các phép tính nặng hoặc tạo mảng/object mới vào sự kiện tần suất cao (`pointermove`, `mousemove`, `scroll`, `resize`, `timeupdate`) nếu có thể chốt dữ liệu từ lúc bắt đầu thao tác hoặc tách lớp CSS riêng.
  - Giữ ổn định tham chiếu dữ liệu/callback và chỉ kích hoạt lifecycle/effect theo đúng giá trị nghiệp vụ thay đổi (tránh render lại toàn bộ danh sách/biểu đồ hoặc làm nhảy vị trí cuộn khi người dùng đang nhập liệu).
- **I/O, Bộ nhớ & Tài nguyên nền**:
  - Xử lý file/dữ liệu lớn theo luồng (stream) hoặc phân trang thay vì nạp toàn bộ vào RAM; tránh sao chép dữ liệu lớn nhiều lần không cần thiết.
  - Gom nhịp (throttle/debounce/checkpoint) khi gọi API tra cứu hoặc ghi dữ liệu trung gian; luôn có timeout và dọn dẹp tài nguyên nền (subscription, stream, tiến trình con, timer, event listener) khi đóng màn hình, hủy thao tác hoặc chuyển ngữ cảnh.

### 3. Tổ chức code và quy trình đọc tốn ít Token nhất cho AI

- **Đọc có mục tiêu (Targeted Reading)**:
  - Trước khi đọc code, tra bảng chỉ mục trong `PROJECT_MAP.md` (hoặc dùng công cụ grep/search theo tên hàm/route/component) để xác định chính xác 1–3 file cần mở.
  - Khi đọc file dài, luôn đọc theo khoảng dòng (`StartLine` – `EndLine`) đúng vùng cần sửa; **cấm đọc toàn bộ các file lớn hoặc nạp toàn bộ repo vào ngữ cảnh** nếu không cần thiết.
  - Với `CHANGELOG.md`, chỉ đọc 40–60 dòng đầu để lấy mẫu và chèn mục phiên bản mới ở đầu file, không đọc toàn bộ lịch sử cũ.
- **Viết code thân thiện với ngữ cảnh AI (Token-Efficient Code)**:
  - Giữ toàn bộ luồng của một chức năng nằm gọn và tuyến tính trong đúng module của nó để AI chỉ cần đọc 1 file là thấy đủ: đầu vào → xử lý → gọi API → cập nhật state (không phải nhảy qua 5–7 file helper/mapper/wrapper nhỏ lẻ).
  - Đặt tên hàm/biến tiếng Việt rõ nghĩa và comment ngắn gọn đúng chỗ hiểm hóc (chỉ giải thích *tại sao* và *ràng buộc ngầm*, không viết comment dài dòng mô tả lại câu lệnh) để giảm số dòng file và giảm lượng token mỗi lần AI đọc.



