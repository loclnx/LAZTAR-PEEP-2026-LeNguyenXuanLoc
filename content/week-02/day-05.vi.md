+++
title = "Ngày 05 - Thứ Sáu - 25/09/2026 (REMOTE) (Tích hợp Sprint 0 và Review cuối)"
weight = 5
+++

## Công việc đã hoàn thành

- Đọc và rà soát yêu cầu cuối cùng của Sprint 0 cho thiết kế Mini-WMS và xác nhận phạm vi tích hợp giữa năm domain chính.
- Kiểm tra các điểm nối giữa Authentication & RBAC, Warehouse Structure, Product & Supplier, Inventory Model và Stock Ledger & Movement.
- Xác định và xử lý các chênh lệch về khóa ngoại, quy ước đặt tên, kiểu dữ liệu, trạng thái và quy tắc nghiệp vụ.
- Chạy kiểm thử các kịch bản vận hành bắt buộc để đảm bảo thiết kế hoạt động thực tế, không chỉ đúng trên giấy.
- Gộp các ERD domain thành ERD tổng v1 và kiểm tra file PlantUML có render thành công hay không.

## Tổng quan

Ngày 05 là ngày tích hợp và review cuối của Sprint 0. Mục tiêu chính không chỉ là hoàn thiện thiết kế từng module, mà là chứng minh rằng cả năm domain có thể hoạt động đồng bộ như một hệ thống WMS thống nhất.

Báo cáo cho thấy thứ 6 là mốc đánh giá chéo giữa các domain. Ở giai đoạn này, nhóm phải chứng minh rằng mô hình có thể xử lý được 8 kịch bản nghiệp vụ, đảm bảo tính truy vết và hỗ trợ vận hành kho mà không làm mất tính nhất quán của dữ liệu. Nói ngắn gọn, thiết kế không chỉ cần đẹp trên sơ đồ mà còn phải đúng với thực tế vận hành.

## Tập trung tích hợp chính

### 1. Đồng bộ giữa các domain

Buổi review xác nhận sự cần thiết của việc thống nhất các mối quan hệ sau:

- Authentication và RBAC: quyền và vai trò của người dùng phải gắn với kho cụ thể.
- Warehouse Structure: mỗi kho và cây vị trí phải hỗ trợ cấu trúc vật lý và quy trình vận hành.
- Product và Supplier: thông tin sản phẩm và nhà cung cấp phải tích hợp đúng với xử lý tồn kho.
- Inventory Model: dữ liệu tồn kho phải được lưu theo vị trí, SKU và lot với các ràng buộc hợp lệ.
- Stock Ledger và Movement: mọi thay đổi tồn kho phải có tính truy vết, nhất quán và đồng bộ với inventory.

Việc đồng bộ này rất quan trọng vì hệ thống không thất bại do một bảng riêng lẻ, mà thường bị lỗi ở các điểm nối giữa các domain.

### 2. Quy tắc nghiệp vụ và chuẩn dữ liệu chung

Checklist xác nhận nhiều điểm bắt buộc cần thống nhất trên toàn hệ thống:

- `user_roles.warehouse_id` phải tham chiếu `warehouses`.
- `stock_ledger.created_by` phải tham chiếu `users`, và vai trò được phép xem hoặc duyệt ledger phải rõ ràng.
- Chỉ `location_type = BIN` mới được lưu inventory, trong khi trạng thái QC và DAMAGED phải được xác định rõ trong logic available.
- Quy tắc UoM và số lượng cơ sở phải thống nhất giữa inventory và ledger.
- Inventory và ledger phải dùng cùng khóa nghiệp vụ và logic giao dịch để đối soát.
- Quy tắc vô hiệu hóa không được cho phép xóa hoặc vô hiệu hóa SKU hoặc vị trí kho vẫn còn đang dùng.

Những quyết định này giúp giảm thiểu tình trạng dữ liệu lệch trạng thái trong quá trình nhận hàng, lưu kho, giao hàng và điều chỉnh.

## Các kịch bản bắt buộc phải kiểm thử

Tài liệu nêu rõ 8 kịch bản nghiệp vụ cần được kiểm tra để xác nhận thiết kế.

### Kịch bản 1: Nhập hàng

Nhập 10 BOX SKU-001 vào `RECV-01`, trong đó 1 BOX = 12 EA.

Kết quả mong đợi:

- Số lượng receipt được ghi đúng.
- Tồn kho tăng 120 EA.
- Số lượng on hand tại vị trí nhận hàng là 120 EA.

### Kịch bản 2: Putaway từ receiving sang storage

Di chuyển 120 EA từ `RECV-01` sang `BIN A01-R03-B05`.

Kết quả mong đợi:

- Ghi hai dòng ledger: -120 và +120.
- Tổng tồn kho không đổi.
- Hàng di chuyển giữa vị trí mà không làm thay đổi tổng lượng.

### Kịch bản 3: Giữ chỗ cho đơn xuất

Tạo đơn xuất 30 EA và giữ chỗ hàng.

Kết quả mong đợi:

- `reserved = 30`.
- `available = 90`.
- Ledger không thay đổi vì reserve là cam kết, không phải là chuyển động vật lý.

### Kịch bản 4: Pick và ship

Pick và ship 30 EA.

Kết quả mong đợi:

- Số lượng on hand giảm còn 90.
- Reserved trở về 0.
- Ledger phản ánh các giao dịch PICK và SHIP.

### Kịch bản 5: Kiểm kê thiếu hàng

Thực hiện kiểm kê và phát hiện thiếu 2 EA.

Kết quả mong đợi:

- Ghi nhận điều chỉnh `ADJUST OUT -2`.
- Nếu cần, quy trình duyệt và kiểm toán phải được áp dụng.

### Kịch bản 6: Receipt sai SKU

Một receipt được tạo với SKU sai.

Kết quả mong đợi:

- Receipt sai được đảo ngược.
- Receipt đúng SKU được ghi lại.
- Không ghi đè sai lịch sử giao dịch cũ.

### Kịch bản 7: Picker không có quyền điều chỉnh

Một picker cố gắng điều chỉnh tồn kho ở khu vực không có quyền.

Kết quả mong đợi:

- Hệ thống từ chối yêu cầu.
- RBAC chặn truy cập trái phép.

### Kịch bản 8: Xung đột giữ chỗ đồng thời

Hai người cùng lúc cố gắng giữ 90 EA cuối cùng.

Kết quả mong đợi:

- Chỉ một giao dịch thành công.
- Giao dịch còn lại bị từ chối bằng cơ chế optimistic locking, versioning hoặc row lock.

Những kịch bản này quan trọng vì chúng kiểm định logic hoạt động thực tế của kho, không chỉ kiểm tra cấu trúc bảng dữ liệu.

## Tiêu chí hoàn thành bắt buộc

Ngày cuối cần đảm bảo các tiêu chí sau:

- ERD tổng v1 render thành công.
- Không có bảng mồ côi trong thiết kế tổng.
- Tất cả khóa ngoại và kiểu dữ liệu liên domain phải khớp với Data Dictionary.
- Tám kịch bản nghiệp vụ phải đi qua review hoặc kiểm thử thực tế.
- Mọi trường hợp không đi qua phải ghi vào danh sách lỗi với người chịu trách nhiệm và kế hoạch xử lý.
- Source PlantUML phải được cập nhật trên Git và sẵn sàng nộp cho bộ phận chấm.

## Rủi ro cần kiểm soát

Báo cáo nêu rõ các rủi ro ưu tiên cần xử lý:

- Sự khác biệt giữa inventory và ledger trong logic reserve.
- Khóa và kiểu dữ liệu không thống nhất giữa các module.
- ERD tổng không render được.

Những vấn đề này được xem là rất quan trọng vì chúng ảnh hưởng trực tiếp đến tính đúng của nghiệp vụ và độ tin cậy của sản phẩm. Vì vậy, nhóm cần đưa chúng thành quyết định cụ thể trong buổi họp tích hợp, thay vì để sang ngày nộp cuối cùng.

## Nhận xét

Ngày 05 cho thấy Sprint 0 không chỉ là vẽ sơ đồ database, mà là chứng minh mô hình có thể vận hành theo đúng các quy trình kho thực tế. Nhóm đã phải kiểm tra ranh giới domain, đối chiếu các điểm nối giữa các bảng và đảm bảo quy tắc nghiệp vụ nhất quán trong các giao dịch, điều chỉnh và phê chuẩn.

Bài học quan trọng nhất là: tích hợp mới là nơi đánh giá chất lượng hệ thống thực sự. Một thiết kế trông hợp lý khi tách riêng từng module vẫn có thể sai khi đưa vào cùng một hệ thống nếu inventory, ledger, RBAC, vị trí và product không đồng bộ. Buổi review này đã giúp đóng lại khoảng trống đó và tạo nền tảng vững chắc cho giai đoạn triển khai tiếp theo.

## Kết luận

Ngày 05 đã hoàn tất buổi review tích hợp cuối Sprint 0 cho dự án Mini-WMS. Nhóm xác nhận các điểm nối giữa năm domain chính, kiểm tra các kịch bản nghiệp vụ bắt buộc và hợp nhất ERD thành bản v1 thống nhất. Kết quả là một thiết kế gần với thực tế vận hành hơn, sẵn sàng cho giai đoạn triển khai và báo cáo cuối cùng.
