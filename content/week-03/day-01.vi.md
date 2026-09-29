+++
title = "Ngày 01 - Thứ hai - 28/09/2026 (REMOTE) Lập kế hoạch Sprint 0 & Đồng bộ domain"
weight = 1
+++

## Công việc đã làm

- Đọc và rà soát kế hoạch Sprint 0 cũng như phạm vi thiết kế của Mini-WMS.
- Thảo luận về năm domain chính: Authentication & RBAC, Warehouse Structure, Product & Supplier, Inventory Model và Stock Ledger & Movement.
- Chốt các quy ước chung về đặt tên, kiểu dữ liệu, cột audit và chính sách xóa mềm cho toàn bộ hệ thống.
- Xác định các quy tắc về khóa chính, khóa ngoại, trạng thái, và metadata bắt buộc cho từng module.
- Bắt đầu phác thảo luồng nghiệp vụ cho các kịch bản nhập hàng, putaway, lấy hàng, giao hàng, chuyển kho và điều chỉnh tồn kho.
- Xác định các điểm phụ thuộc giữa các domain để thiết kế được gắn kết với nhau thay vì làm riêng rẽ.

## Tổng quan

Ngày 01 của Sprint 0 tập trung vào nền tảng thiết kế hệ thống. Mục tiêu trọng tâm không phải là thiết kế từng domain riêng lẻ, mà là xây dựng một kiến trúc WMS thống nhất trong đó tất cả module đều phù hợp về tên gọi, logic và quy tắc nghiệp vụ.

Đây là giai đoạn chuyển từ hiểu biết cấp cao sang lập kế hoạch cấu trúc thực tế. Nhóm phải quyết định các chuẩn mực sẽ điều khiển ERD, data dictionary và business rules sau này. Nếu không đồng bộ ngay từ đầu, thiết kế sẽ dễ phát sinh sai lệch và khó triển khai.

## Các nội dung trọng tâm

### 1. Mục tiêu của Sprint 0

Kế hoạch dự án nêu rõ rằng Sprint 0 phải tạo ra một bộ thiết kế tích hợp cho hệ thống quản lý kho. Đầu ra cuối cùng không chỉ là các bảng riêng lẻ, mà là một cấu trúc thiết kế đủ mạnh để hỗ trợ các nghiệp vụ kho thật tế.

Các deliverable chính bao gồm:

- Sơ đồ luồng nghiệp vụ cho các tình huống hoạt động.
- ERD v1 của toàn hệ thống.
- Data dictionary theo từng domain.
- Danh sách business rules có mã định danh.

### 2. Phân chia domain

Nhóm chia hệ thống thành 5 domain với phạm vi rõ ràng và chuỗi phụ thuộc hợp lý:

- Domain A: Authentication & RBAC
- Domain B: Warehouse Structure
- Domain C: Product & Supplier
- Domain D: Inventory Model
- Domain E: Stock Ledger & Movement

Cách chia này rất quan trọng vì thiết kế không thể tách biệt giữa các module. Ví dụ, cấu trúc kho và dữ liệu sản phẩm là nền tảng trước khi có thể mô hình hóa tồn kho và ledger đúng cách.

### 3. Các quy ước chung đã thống nhất ngày 1

Nhóm đã đồng thuận các chuẩn mực sau để áp dụng cho toàn bộ database design:

- Tên bảng theo dạng snake_case và số nhiều.
- Tên cột theo dạng snake_case.
- Khóa chính dùng `id`, khóa ngoại dùng `<table>_id`.
- Giá trị trạng thái lưu dưới dạng mã chữ hoa như ACTIVE, INACTIVE, RECEIPT, SHIP.
- Trường số lượng dùng `DECIMAL(18,4)` thay vì kiểu float.
- Thời gian lưu theo UTC, sau đó chuyển theo múi giờ của người dùng khi hiển thị.
- Bảng master bắt buộc có cột audit: `created_at`, `created_by`, `updated_at`, `updated_by`.
- Dữ liệu ledger chỉ ghi thêm, không sửa/xóa trực tiếp lịch sử.

Những quy ước này cực kỳ quan trọng vì dữ liệu kho phải đảm bảo tính chính xác, truy vết và kiểm toán.

### 4. Phụ thuộc và logic tích hợp

Kế hoạch cũng nhấn mạnh rằng B và C là hai domain nền tảng. Hai domain này phải được “khóa” sớm vì D và E phụ thuộc trực tiếp vào chúng. Nói cách khác, cấu trúc kho và dữ liệu sản phẩm phải xác định rõ trước khi thiết kế logic tồn kho và biến động hàng hóa.

Đây là bài học giúp nhóm nhận ra rằng chất lượng thiết kế phụ thuộc vào cách các domain nối với nhau, không chỉ vào bảng riêng lẻ.

### 5. Luồng nghiệp vụ và kịch bản kiểm thử

Nhóm bắt đầu phác thảo vòng đời kho bao gồm:

- Nhập hàng
- Putaway vào vị trí lưu trữ
- Giữ chỗ cho đơn xuất
- Lấy hàng và giao hàng
- Chuyển kho nội bộ
- Điều chỉnh tồn kho và kiểm kê

Trong checklist Sprint 0 còn có 8 kịch bản nghiệp vụ bắt buộc phải kiểm thử trên giấy để xác thực thiết kế. Những tình huống này bao gồm nhập hàng đúng SKU, putaway hợp lệ, giữ chỗ tồn kho, điều chỉnh thiếu hụt, đảo bút toán sai SKU, chặn hành động theo RBAC và xung đột giữ chỗ đồng thời.

## Bài học rút ra

### 1. Đồng bộ nhóm quan trọng hơn mô hình riêng lẻ

Một domain có thể thiết kế rất tốt nhưng vẫn fail khi ghép với các module khác. Bài học lớn nhất của ngày 1 là dự án yêu cầu một “hợp đồng chung” trên cả 5 domain.

### 2. Quy tắc tồn kho là phần phức tạp nhất

Cuộc thảo luận cho thấy các biến động tồn kho không phải là thao tác CRUD đơn giản. Hệ thống phải theo dõi tồn vật lý, tồn dự trữ và tồn khả dụng đồng thời giữ cho ledger và trạng thái nghiệp vụ luôn nhất quán.

### 3. Tính truy xuất là bắt buộc

Dự án không chỉ lưu dữ liệu, mà còn cần giữ vết audit cho toàn bộ hoạt động. Mỗi giao dịch, chỉnh sửa, phê duyệt hoặc đảo bút toán đều phải có dấu vết rõ ràng.

### 4. Quy ước chuẩn hóa giảm rủi ro

Những quyết định về đặt tên, trạng thái, đơn vị tính và cột audit nghe nhỏ nhưng lại giúp giảm rất nhiều lỗi tích hợp trong giai đoạn sau.

## Những thách thức đã thảo luận

### 1. Rủi ro lệch chuẩn giữa các domain

Một rủi ro lớn là các thành viên cùng định nghĩa một trường hoặc khái niệm khác nhau, dẫn đến khóa ngoại không khớp hoặc logic nghiệp vụ không thống nhất.

Giải pháp là chốt quy ước sớm và review lại phụ thuộc giữa các domain trong buổi họp tích hợp.

### 2. Khó khăn với hành vi reserve

Sprint 0 cũng nêu rõ một quyết định quan trọng: có ghi hành động giữ chỗ vào stock ledger hay chỉ để trong inventory model. Điều này trực tiếp ảnh hưởng đến cách triển khai tính truy vết và audit.

### 3. Áp lực thời gian và phụ thuộc

Vì domain B và C phải hoàn thành sớm, nhóm phải duy trì nhịp làm việc chặt chẽ. Nếu cấu trúc kho hoặc mô hình sản phẩm bị trễ, các phần inventory và ledger sẽ khó đi tiếp.

## Kết luận

Ngày 01 của tuần 3 đặt nền móng cho toàn bộ công việc thiết kế Sprint 0. Nhóm đã thống nhất kiến trúc hệ thống, xác định người phụ trách từng domain và chốt các quy tắc định hướng cho mô hình dữ liệu và logic tồn kho.

Đây là bước bắt đầu mạnh vì dự án tập trung vào việc đồng bộ và tích hợp ngay từ đầu, thay vì lao vào thiết kế module riêng lẻ rồi mới phát hiện chúng không khớp nhau. Kết quả là quá trình thiết kế Mini-WMS trở nên thực tế và bền vững hơn.
