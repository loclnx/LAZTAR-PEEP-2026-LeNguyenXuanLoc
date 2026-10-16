* **Chống tạo trùng lệnh cất hàng:** Ẩn nút tạo lệnh khi phiếu nhập đã có lệnh cất hàng; server cũng chặn tạo trùng.
* **Sửa/xóa phiếu nhập:** Thêm API sửa và xóa, bổ sung nút sửa trên giao diện. Chỉ cho phép trước khi quản lý duyệt; server kiểm tra lại khi thao tác.
* **Tạo phiếu ở bộ lọc “Tất cả kho”:** Bỏ disable nút tạo phiếu nhập, cho chọn kho nhận trong form.
* **Bắt buộc hạn soạn hàng:** Không cho tiếp tục/lưu đơn xuất khi chưa nhập hạn hoàn tất soạn hàng.
* **Chặn xác nhận nhập kho trước duyệt:** Phiếu phải được phân công và quản lý duyệt; chỉ nhân viên được giao mới được xác nhận.
* **Bổ sung phiếu nhập vào “Đơn duyệt”:** Quản lý/admin nhìn thấy cả phiếu chưa phân công. Thêm trạng thái rõ ràng và nút  **Phân công & duyệt** .
* **Xử lý thiếu toàn bộ hàng:** Thêm option  **Hủy toàn bộ đơn hàng** , đồng bộ trạng thái và giải phóng hàng giữ. Khóa phương án **Giao phần có sẵn** khi toàn đơn không có hàng để giao, đồng thời chặn ở server.

+++
title = "Ngày 04 - Thứ năm - 15/10/2026 (ON-SITE)"
weight = 4
+++

## Công việc đã làm

- **Kiểm thử và rà soát các hạng mục đồng bộ hệ thống**:
  - Rà soát thao tác xem chi tiết và giao diện theo quyền; bảo đảm các thao tác, tab và menu được hiển thị hoặc vô hiệu hóa đúng quyền.
  - Kiểm tra các bước xử lý đơn xuất theo quyền, luồng giao và đổi người thực hiện, cũng như quyền thao tác sau khi phân công.
  - Rà soát danh sách công việc theo người được giao và phạm vi kho.
  - Kiểm tra validation ngày sản xuất, hạn sử dụng; cảnh báo vượt sức chứa; thông báo lỗi và cách hiển thị kết quả cân.
  - Rà soát thứ tự dữ liệu mới nhất trong các API danh sách, lựa chọn nhà vận chuyển và giao diện trang Điều chuyển.
  - Kiểm tra tính nhất quán của form chọn mặt hàng, badge trạng thái và thẻ thống kê trên desktop/mobile.
  - Kiểm tra việc loại vị trí đang kiểm kê khỏi các lựa chọn và chặn thao tác trên phiếu đã mở nếu vị trí không còn hợp lệ.

## Tổng kết

Ngày làm việc tại văn phòng tập trung rà soát các yêu cầu về phân quyền, phân công, validation và tính nhất quán giữa các màn hình. Các hạng mục bao quát từ luồng đơn xuất, danh sách công việc, nhập/xuất và điều chuyển đến trạng thái kiểm kê, cảnh báo và hiển thị thống kê.

## Kế hoạch ngày mai

- Tiếp tục kiểm thử các hạng mục đã rà soát và ghi nhận vấn đề cần điều chỉnh.
- Kiểm tra lại các luồng nghiệp vụ sau khi có cập nhật.
