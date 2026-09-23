+++
title = "Ngày 03 - 23/09/2026 (Thiết kế cấu trúc kho)"
weight = 3
+++

## Công việc đã hoàn thành

- Đọc và nghiên cứu tài liệu triển khai cấu trúc kho cho Mini-WMS.
- Tìm hiểu mô hình warehouse và locations như nền tảng cho việc quản lý tồn kho.
- Làm rõ cấu trúc tự tham chiếu của vị trí kho và các quy tắc nghiệp vụ liên quan.
- Xác định các ràng buộc cho picking, lưu trữ và truy vết hàng tồn kho.
- Vẽ và rà soát business flow cho cấu trúc kho và các hoạt động liên quan.

## Tổng quan

Ngày hôm nay tập trung vào domain cấu trúc kho, đây là nền tảng để theo dõi tồn kho theo vị trí, quản lý BIN, và đảm bảo truy vết trong quá trình nhận hàng, cất hàng, soạn hàng, QC và xử lý hàng hỏng trong Mini-WMS.

## Quyết định thiết kế chính

### 1. Mô hình warehouse và location

Hệ thống sử dụng hai bảng chính là `warehouses` và `locations`. Bảng `locations` được thiết kế dưới dạng tự tham chiếu, giúp biểu diễn chuỗi vị trí thực tế trong kho mà không cần tách riêng từng bảng cho zone, aisle, rack và bin.

### 2. Cấu trúc vị trí chuẩn

Cấu trúc chuẩn được thống nhất như sau:

`WAREHOUSE -> ZONE -> AISLE -> RACK -> BIN`

Cách này dễ hiểu cho vận hành kho và có thể mở rộng trong tương lai. Chỉ ở cấp BIN mới được dùng để chứa tồn kho.

### 3. Quy tắc picking và trạng thái

Không phải vị trí nào cũng được phép dùng để soạn hàng. Chỉ các BIN có mục đích phù hợp và `is_pickable = true` mới được chọn để picking. Các khu vực như QC, hàng hỏng, nhận hàng và tập kết không được dùng cho hoạt động stock allocation bình thường.

## Quy tắc nghiệp vụ

- `BR-B-01`: mã vị trí phải duy nhất trong cùng kho.
- `BR-B-02`: vị trí cha và con phải thuộc cùng warehouse.
- `BR-B-03`: cấu trúc cây phải đúng theo thứ tự `ZONE -> AISLE -> RACK -> BIN`.
- `BR-B-04`: tồn kho chỉ được ghi nhận ở mức `BIN`.
- `BR-B-05`: BIN ở QC, hỏng, nhận hàng, tập kết không được phép pickable.
- `BR-B-06`: không được vô hiệu hóa vị trí khi còn hàng tồn trong đó.
- `BR-B-07`: dữ liệu warehouse và location không nên bị xóa cứng sau khi đã có giao dịch phát sinh.

## Tích hợp với các domain khác

Cấu trúc kho có liên kết chặt chẽ với các phần hệ thống khác:

- Authentication & RBAC: quyền truy cập được giới hạn theo kho.
- Inventory model: mỗi bản ghi tồn kho phải tham chiếu đến BIN hợp lệ trong cùng warehouse.
- Stock ledger: mọi bút toán putaway, chuyển vị trí và điều chỉnh phải phù hợp với cây vị trí kho.
- Product & Supplier: cấu trúc kho giúp truy vết hàng hóa mà không tạo phụ thuộc trực tiếp quá chặt.

## Sơ đồ business flow

Sơ đồ business flow cho cấu trúc kho và luồng vận hành đã được vẽ và rà soát trong buổi học hôm nay.

- Link sơ đồ: [Business Flow Diagram](https://app.diagrams.net/#G1Vc7Tz9084zgDJpeSOipud6UmZ0-48kWH#%7B%22pageId%22%3A%223coEni-nVhuQpga9hQnN%22%7D)

## Nhận xét

Ngày 03 cho thấy cấu trúc kho không chỉ là một bảng dữ liệu đơn giản, mà là xương sống của hệ thống tồn kho. Nếu mô hình vị trí được thiết kế đúng, nhóm sẽ dễ kiểm soát lượng hàng, ngăn chọn sai vị trí, và tăng tính truy vết cho toàn bộ quy trình vận hành.

## Kết luận

Ngày hôm nay hoàn tất phần thiết kế cấu trúc kho cho Mini-WMS. Nhóm thống nhất về cấu trúc phân cấp rõ ràng, quy tắc tồn kho chặt chẽ và các điểm tích hợp cần thiết cho việc triển khai tiếp theo trong inventory, stock ledger và vận hành kho.
