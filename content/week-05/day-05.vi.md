+++
title = "Ngày 05 - Thứ sáu - 16/10/2026 (ON-SITE)"
weight = 5
+++

## Công việc đã làm

- **Chống tạo trùng lệnh cất hàng**:
  - Ẩn nút tạo lệnh cất hàng khi phiếu nhập đã có lệnh.
  - Bổ sung kiểm tra phía máy chủ để ngăn tạo lệnh trùng.
- **Sửa và xóa phiếu nhập**:
  - Bổ sung API sửa và xóa phiếu nhập cùng nút sửa trên giao diện.
  - Chỉ cho phép thao tác trước khi quản lý duyệt; máy chủ kiểm tra điều kiện này khi xử lý yêu cầu.
- **Tạo phiếu nhập từ bộ lọc “Tất cả kho”**:
  - Bỏ trạng thái vô hiệu hóa nút tạo phiếu nhập.
  - Cho phép chọn kho nhận trực tiếp trong biểu mẫu.
- **Bắt buộc hạn hoàn tất soạn hàng**:
  - Không cho tiếp tục hoặc lưu đơn xuất nếu chưa nhập hạn hoàn tất soạn hàng.
- **Chặn xác nhận nhập kho trước khi duyệt**:
  - Yêu cầu phiếu phải được phân công và quản lý duyệt trước khi xác nhận nhập kho.
  - Chỉ nhân viên được phân công mới được xác nhận.
- **Bổ sung phiếu nhập vào “Đơn duyệt”**:
  - Hiển thị cả phiếu chưa phân công cho quản lý và admin.
  - Bổ sung trạng thái rõ ràng và nút **Phân công & duyệt**.
- **Xử lý trường hợp thiếu toàn bộ hàng**:
  - Bổ sung lựa chọn **Hủy toàn bộ đơn hàng**, đồng bộ trạng thái và giải phóng hàng đang giữ.
  - Khóa lựa chọn **Giao phần có sẵn** khi không có hàng nào có thể giao; máy chủ cũng từ chối phương án này.

## Tổng kết

Ngày làm việc tại văn phòng tập trung hoàn thiện các luồng nhập kho, duyệt phiếu và xử lý đơn xuất. Các thay đổi bổ sung kiểm tra ở cả giao diện lẫn máy chủ nhằm ngăn thao tác trùng hoặc không hợp lệ, đồng thời làm rõ quy trình phân công, phê duyệt và xử lý khi thiếu hàng.

## Kế hoạch ngày mai

- Tiếp tục kiểm thử các luồng tạo, sửa, xóa và duyệt phiếu nhập.
- Rà soát tính nhất quán trạng thái đơn hàng và tồn kho sau các phương án xử lý thiếu hàng.
