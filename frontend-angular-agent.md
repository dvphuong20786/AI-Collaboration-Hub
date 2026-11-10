# Quy ước riêng cho frontend Angular

> Bổ sung cho `AGENTS.md`. Đọc `AGENTS.md` trước; các quy tắc dưới đây chỉ áp dụng cho code Angular và chỉ bổ sung cho đúng phạm vi ghi trong từng mục — không thay quy tắc chung.

## FormGroup và state đơn giản

- Với form nhỏ và vừa, mặc định ưu tiên biến TypeScript thông thường kết hợp `[ngModel]`, `(ngModelChange)` hoặc `[(ngModel)]` để dễ nhìn thấy luồng dữ liệu.
- Không mặc định dùng `FormGroup` chỉ vì đây là dự án Angular.
- Chỉ dùng `FormGroup` khi form thật sự lớn, validation phức tạp, nhiều bước hoặc cần quản lý đồng bộ nhiều trạng thái của field.
- Với popup cấu hình rất đơn giản như danh sách switch/ID được chọn, có thể dùng biến và `Set<number>` trực tiếp để code dễ đọc hơn.
- Không dùng `FormGroup` chỉ để bọc một giá trị đơn giản nếu điều đó làm luồng khó hiểu hơn.
- Nếu dùng `FormGroup`, khai báo typed controls rõ ràng và tránh truy cập bằng chuỗi lặp lại như `controls['fieldName']` ở nhiều nơi; không tạo getter chỉ để che thao tác đọc control.
- Nếu một form dùng `FormGroup`, phải có comment ngắn giải thích lý do cần dùng thay vì các biến thông thường.
- Khi không dùng `FormGroup`, validation phải nằm trong các hàm có tên rõ nghĩa như `validateForm()`, `isFormValid()` và payload phải được tạo ở `buildSaveRequest()`.
- Một giá trị nghiệp vụ chỉ được lưu theo một kiểu dữ liệu thống nhất trong toàn bộ luồng.
- Filter chọn đơn như `[nzFilterMultiple]="false"` phải lưu trực tiếp một giá trị hoặc `null`; không lưu thành mảng và không viết thêm `Array.isArray()` để hỗ trợ đồng thời hai kiểu dữ liệu.
- Ví dụ trạng thái chọn đơn: chọn thì lưu `tripStatus = status`, bỏ chọn hoặc xóa lọc thì lưu `tripStatus = null`.

Ví dụ filter chọn đơn dễ hiểu:

```ts
protected onSelectTripStatus(status: TripStatus): void {
  const currentStatus = this.searchForm.get('tripStatus')?.value;

  // Bấm lại trạng thái đang chọn thì bỏ lọc trạng thái.
  if (currentStatus === status) {
    this.searchForm.get('tripStatus')?.setValue(null);
    return;
  }

  // Bấm trạng thái khác thì thay bằng trạng thái mới.
  this.searchForm.get('tripStatus')?.setValue(status);
}
```

Không viết filter chọn đơn thành nhiều kiểu dữ liệu:

```ts
const statuses = Array.isArray(tripStatus) ? tripStatus : [tripStatus];
const selectedStatus = statuses[0];
```

## Luồng dữ liệu của component và thẻ custom

- Các thẻ không phải HTML mặc định như `app-*`, `app-select`, `app-date-picker` phải được viết sao cho có thể hiểu luồng dữ liệu ngay tại template cha.
- Input truyền xuống và event trả lên phải có tên cụ thể theo nghiệp vụ; không dùng các tên mơ hồ như `data`, `model`, `changed`, `handleChange` nếu có thể đặt tên rõ hơn.
- Với component nhập liệu dùng chung, ưu tiên cặp tên đối xứng như `[value]` và `(valueChange)`.
- Với component mang nghiệp vụ cụ thể, dùng tên rõ như `[selectedBankId]` và `(bankSelected)`.
- Event trong HTML chỉ gọi một hàm có tên rõ nghĩa; không gán nhiều biến hoặc gọi nhiều hàm trực tiếp trong biểu thức event.
- Component nhập liệu con chỉ hiển thị, nhận giá trị và phát sự kiện. Không được âm thầm gọi API nghiệp vụ không thuộc trách nhiệm của nó.
- API và các thay đổi field phụ thuộc nên nằm ở component form cha để mở file cha là thấy được toàn bộ luồng nghiệp vụ.
- Không tạo nhiều lớp component bọc nhau cho một field. Thông thường chỉ nên có `Form nghiệp vụ → Component nhập liệu → app-select/input`.
- Nếu component chỉ bọc lại `app-select` hoặc `input` nhưng không có nghiệp vụ, style dùng chung hoặc hành vi đáng kể thì dùng trực tiếp `app-select`/`input`.
- Với component custom phức tạp, thêm comment `Luồng dữ liệu` ở đầu class, chỉ rõ dữ liệu nào đi xuống Input, sự kiện nào đi lên Output và API được gọi tại đâu.
- Khi dùng thẻ thư viện, viết tách dòng các binding quan trọng như giá trị, loading, disabled và sự kiện thay đổi để dễ đọc.
- Không dùng two-way binding tùy chỉnh dạng `[(value)]` nếu nó làm khó tìm điểm xử lý. Ưu tiên viết rõ `[value]` và `(valueChange)` khi cần theo dõi luồng.

Ví dụ dễ theo dõi:

```html
<app-bank-selector
  [banks]="banks"
  [selectedBankId]="selectedBankId"
  [disabled]="isBankInformationReadonly"
  (bankSelected)="onBankSelected($event)"
/>
```

```ts
protected onBankSelected(bankId: number): void {
  this.selectedBankId = bankId;
  this.accountNumber = '';
  this.accountName = '';
}
```

Nhìn từ template phải lần được luồng: `selectedBankId` truyền xuống component, người dùng chọn ngân hàng, `bankSelected` phát lên và `onBankSelected()` xử lý.

## Change detection và gán state

- Không mutate constant hoặc object dùng chung rồi dùng `cloneDeep()` để buộc Angular cập nhật. Component phải có state hiển thị riêng và tạo reference mới ngay tại hàm cập nhật state đó.
- Không dựa vào Angular tự render, change detection hoặc Input thay đổi mà bỏ qua đoạn gán state. Phải nhìn thấy rõ câu lệnh gán state nào khiến phần HTML nào cập nhật.

## RxJS finalize và loading

- Được dùng RxJS `finalize()` và nên giữ `finalize()` để tắt loading tại một chỗ cho cả thành công lẫn lỗi.
- Khi dùng `finalize()`, đặt nó sát lời gọi API và thêm comment `// finalize chạy khi API thành công hoặc lỗi` nếu người đọc có thể không quen RxJS.
- Không tắt cùng một biến loading vừa trong `next`, vừa trong `error`, vừa trong `finalize()`. Chỉ để một điểm tắt tại `finalize()`.
- Khi dùng `finalize()` để tắt loading phải comment ngắn rằng nó chạy cho cả thành công và lỗi, nếu người đọc khó nhận ra điều đó.

## @Input, @ViewChild và ngOnChanges

- Không dựa vào việc một hoặc nhiều `@Input` tình cờ thay đổi để âm thầm gọi API. Cách này dễ lỗi khi người dùng thực hiện lại cùng điều kiện và rất khó lần luồng.
- Nếu component cha yêu cầu component con tải dữ liệu, cha dùng `@ViewChild` và gọi trực tiếp một hàm có tên rõ nghĩa như `searchReport(searchParams)`.
- Không dùng biến đếm `requestNumber`, setter hoặc `ngOnChanges()` để ngầm kích hoạt API.
- Không tạo hành vi phụ khó thấy bên trong getter, setter, `ngOnChanges()` hoặc subscription dùng chung. Nếu bắt buộc dùng lifecycle hook, điều kiện kích hoạt phải được đặt thành biến có tên rõ nghĩa và comment lý do.

Ví dụ luồng dễ theo dõi:

```html
<app-bc-tc-02
  [rows]="platformRows"
  (rowsChange)="onPlatformRowsChange($event)"
  (loadingChange)="onPlatformLoadingChange($event)"
/>
```

```ts
private searchPlatformFeeReport(): void {
  this.bcTc02ReportComponent?.searchReport({
    intervalType: this.selectedInterval,
    startDate: this.appliedFromDate,
    endDate: this.appliedToDate,
  });
}
```

## Comment cho đoạn Angular đặc biệt

- Các đoạn dùng `OnPush`, `markForCheck`, `inject`, RxJS hoặc xử lý trạng thái đặc biệt phải có giải thích ngắn nếu luồng không hiển nhiên.

## Template và style Angular

- Giữ template gọn nhưng vẫn thụt dòng để thấy rõ phạm vi `@if`, `@for`, `@switch`.
- Style của bảng và bảng chi tiết dùng `share-table` phải đặt trong component `share-table`; màn hình nghiệp vụ chỉ cấu hình cột, dữ liệu, dòng tổng và nội dung ô đặc thù, không chép lại CSS cấu trúc bảng.
- CSS dùng chung của các báo cáo đang hoạt động đặt tại `src/app/pages/report/report.css`; không áp dụng quy tắc gom CSS cho folder có hậu tố `.old`.

## Đặt tên component báo cáo

- Tên folder, file, class và selector của component báo cáo phải bám theo mã báo cáo để tìm trực tiếp, ví dụ `RP-01/rp-01.component.ts`, `Rp01Component`, `app-rp-01`.
