+++
title = "Ngày 04 - 24/09/2026 (Triển khai ERD cấu trúc kho)"
weight = 4
+++

## Công việc đã hoàn thành

- Đọc và rà soát tài liệu triển khai ERD cho domain Warehouse Structure trong Mini-WMS.
- Xác nhận mô hình dữ liệu cốt lõi gồm hai bảng `warehouses` và `locations`.
- Làm rõ cấu trúc phân cấp vị trí kho theo dạng tự tham chiếu với `parent_id`.
- Xác định các loại vị trí hợp lệ và mục đích vận hành của từng cấp kho.
- Định nghĩa các ràng buộc dữ liệu, chỉ mục và điểm tích hợp với inventory, stock ledger và RBAC.

## Tổng quan

Ngày hôm nay tập trung vào việc chuyển thiết kế cấu trúc kho sang mô hình ERD có thể triển khai thực tế. Tài liệu xác định cách hệ thống biểu diễn kho, vị trí vật lý và mối quan hệ phân cấp giữa các khu vực. Mục tiêu là xây dựng nền tảng ổn định cho việc kiểm soát tồn kho, phân bổ hàng, vận chuyển hàng và truy vết kho.

Thiết kế sử dụng hai bảng chính: `warehouses` và `locations`. Cách này vừa đơn giản vừa mạnh vì nó cho phép mô hình hóa cấu trúc kho thực tế đồng thời vẫn dễ mở rộng trong tương lai.

## Quyết định triển khai chính

### 1. Mô hình kho cốt lõi

Bảng `warehouses` là bản ghi chính của từng kho vận hành. Bảng này lưu mã kho, tên hiển thị, địa chỉ, trạng thái và thông tin theo dõi người tạo, người cập nhật.

Những điểm quan trọng:

- `code` phải là duy nhất và không nên thay đổi sau khi đã phát sinh giao dịch.
- `status` nên giữ ở dạng `ACTIVE` hoặc `INACTIVE` thay vì xóa bản ghi cứng.
- Một kho có thể chứa nhiều khu vực vận hành như nhận hàng, lưu trữ, soạn hàng, tập kết, QC và khu hàng hỏng.

### 2. Cây vị trí tự tham chiếu

Bảng `locations` là bảng tự tham chiếu và lưu cả cấu trúc vật lý và mục đích vận hành của từng vị trí trong kho.

Cấu trúc vị trí được mô hình hóa như sau:

`WAREHOUSE -> ZONE -> AISLE -> RACK -> BIN`

Cách này cho phép mô tả cấu trúc kho thật sự mà không cần tạo quá nhiều bảng riêng lẻ. Trường `parent_id` liên kết vị trí con với vị trí cha, trong khi `warehouse_id` đảm bảo mỗi vị trí thuộc đúng một kho.

### 3. Quy tắc lưu trữ: chỉ BIN mới chứa tồn kho

Một quyết định quan trọng là tồn kho không được ghi trực tiếp ở cấp `ZONE`, `AISLE` hoặc `RACK`. Chỉ `location_type = BIN` mới được phép làm vị trí cuối cùng chứa hàng tồn.

Điều này tạo ra logic vận hành rõ ràng:

- `ZONE` và `AISLE` định nghĩa cấu trúc vật lý.
- `RACK` tổ chức không gian lưu trữ.
- `BIN` là đơn vị thực sự chứa sản phẩm.

## Ghi chú mô hình dữ liệu và ERD

### `warehouses`

Bảng `warehouses` lưu thông tin master kho, bao gồm:

- `id`: định danh nội bộ của kho.
- `code`: mã nghiệp vụ như `WH01`.
- `name`: tên kho.
- `address`: địa chỉ kho.
- `status`: trạng thái hoạt động.
- `created_at` và `updated_at`: thời gian tạo và cập nhật.
- `created_by` và `updated_by`: người tạo và người cập nhật.

### `locations`

Bảng `locations` lưu toàn bộ cấu trúc vị trí vật lý và mục đích vận hành, bao gồm:

- `id`: định danh của vị trí.
- `warehouse_id`: kho sở hữu vị trí.
- `parent_id`: tham chiếu vị trí cha.
- `location_type`: `ZONE`, `AISLE`, `RACK`, hoặc `BIN`.
- `code`: mã vị trí duy nhất trong kho.
- `full_path`: đường dẫn đầy đủ như `WH01/Z-A/A01/R03/B05`.
- `purpose`: `RECEIVING`, `STORAGE`, `PICKING`, `STAGING`, `QC`, hoặc `DAMAGED`.
- `max_weight` và `max_volume`: giới hạn tải trọng và dung tích.
- `is_pickable`: cho biết BIN có được dùng cho picking không.
- `is_active`: trạng thái hoạt động để hỗ trợ soft delete.

## Quy tắc nghiệp vụ

Thiết kế xác định các quy tắc quan trọng sau:

- `BR-WH-01`: mã vị trí phải duy nhất trong cùng kho.
- `BR-WH-02`: vị trí cha và con phải thuộc cùng warehouse.
- `BR-WH-03`: cấu trúc phân cấp phải theo đúng thứ tự `ZONE -> AISLE -> RACK -> BIN`.
- `BR-WH-04`: chỉ vị trí ở cấp `BIN` mới được tham chiếu bởi inventory.
- `BR-WH-05`: BIN ở QC, DAMAGED, RECEIVING và STAGING mặc định `is_pickable = false`.
- `BR-WH-06`: không được vô hiệu hóa vị trí khi nó vẫn còn hàng tồn hoặc đang được tham chiếu bởi giao dịch mở.
- `BR-WH-07`: dữ liệu warehouse và location không nên bị xóa cứng sau khi đã phát sinh giao dịch.

## Chỉ mục và ràng buộc đề xuất

ERD cũng đề xuất một số chỉ mục và kiểm tra hợp lệ:

- Unique constraint trên `(warehouse_id, code)`.
- Index trên `(warehouse_id, parent_id)` để duyệt cây vị trí nhanh.
- Index trên `(warehouse_id, location_type, is_active)` để tìm BIN hoạt động hiệu quả.
- Index trên `full_path` để tra cứu nhanh theo đường dẫn vị trí.
- Kiểm tra logic để ngăn parent hierarchy không hợp lệ và vòng lặp tham chiếu.

Những ràng buộc này rất quan trọng bởi vì dữ liệu vị trí kho không chỉ là một danh sách đơn giản, mà là một cây cấu trúc phải luôn nhất quán trong mọi thao tác tồn kho.

## Tích hợp với các domain khác

Cấu trúc kho liên kết với các module khác như sau:

- Authentication & RBAC: mỗi vai trò người dùng gắn với kho cụ thể.
- Inventory model: mỗi bản ghi tồn kho phải tham chiếu đến BIN hợp lệ trong cùng warehouse.
- Stock ledger: mọi bút toán kho đều phải tham chiếu vị trí hợp lệ và giữ tính truy vết.
- Product & Supplier: sản phẩm được đặt vào stock thông qua BIN, không gắn trực tiếp vào kho master.

Thiết kế này giúp hệ thống modular, dễ kiểm soát và không tạo phụ thuộc trực tiếp không cần thiết giữa các module.

## Ví dụ luồng xử lý

Một ví dụ trong tài liệu cho thấy kho `WH01` có cấu trúc:

`Zone Z-A -> Aisle A01 -> Rack R03 -> Bin B05`

`full_path` của BIN này là:

`WH01/Z-A/A01/R03/B05`

Khi nhập 120 kg hàng, hệ thống có thể đặt hàng vào một RECEIVING BIN trước. Sau khi putaway, tồn kho sẽ được chuyển từ vị trí nhận hàng sang BIN lưu trữ. Tổng lượng hàng không đổi, chỉ thay đổi vị trí và trạng thái của hàng trong quy trình kho.

Ví dụ này cho thấy tầm quan trọng của việc mô hình hóa cây kho và logic di chuyển hàng đúng cách.

## Nhận xét

Ngày 04 cho thấy thiết kế cấu trúc kho không chỉ là việc tạo bảng dữ liệu. Đây là cách định nghĩa các quy tắc vận hành để làm cho việc di chuyển hàng, truy vết và kiểm soát kho trở nên khả thi. Thiết kế này tạo ra nền tảng nhất quán cho các phần công việc tiếp theo trong quản lý tồn kho, stock ledger và quy trình vận hành kho.

Bài học quan trọng nhất là một mô hình vị trí sạch sẽ sẽ giảm sai sót trong hoạt động. Nếu cấu trúc vị trí, mục đích sử dụng và quy tắc tồn kho được thiết kế đúng, phần còn lại của hệ thống kho sẽ đáng tin cậy và dễ triển khai hơn.

## Kết luận

Ngày hôm nay đã hoàn tất phần rà soát mức triển khai của ERD cấu trúc kho. Nhóm thống nhất về việc dùng `warehouses` và `locations`, cây tự tham chiếu, quy tắc nghiêm ngặt về BIN và nhu cầu có các kiểm tra dữ liệu và soft-delete. Những quyết định này tạo ra nền tảng vững chắc và thực tế cho domain inventory của Mini-WMS.
