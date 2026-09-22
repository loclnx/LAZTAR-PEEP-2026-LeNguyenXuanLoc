+++
title = "Ngày 01 - 21/09/2026 (ON-SITE) Tổng quan dự án"
weight = 1
+++

## Công việc đã làm

- Xem lại overview của dự án Mini-WMS.
- Nghiên cứu quy trình kho từ nhận hàng đến hoàn trả.
- Xác định các quy tắc nghiệp vụ, yêu cầu vận hành và công nghệ dự kiến được sử dụng.
- Thảo luận về cách dự án quản lý tồn kho, lô hàng, phân bổ theo FEFO và truy xuất nguồn gốc sản phẩm.

## Tổng quan dự án

Dự án này tập trung vào việc xây dựng một Hệ thống Quản lý Kho Mini cho FreshLink Produce, một doanh nghiệp giả định chuyên về thực phẩm tươi sống. Mục tiêu chính là mô phỏng quy trình vận hành thực tế của một kho hàng tươi sống, nơi chất lượng, độ tươi, truy xuất lô hàng, số lượng tồn kho chính xác và xử lý đơn hàng đúng thời hạn là những yếu tố then chốt.

Hệ thống dự kiến sẽ hỗ trợ một chu trình kho hoàn chỉnh: Nhập hàng → Bày xếp vào vị trí → Phân bổ tồn kho → Lấy hàng → Đóng gói → Giao hàng → Hoàn trả.

## Những điểm quan trọng đã học

### 1. Quy trình kho

Quy trình vận hành bắt đầu từ việc nhận hàng, sau đó đưa hàng vào vị trí lưu trữ, phân bổ hàng cho đơn khách, và cuối cùng là chuẩn bị giao hàng. Các mặt hàng trả lại cũng sẽ được xử lý quay lại chu trình tồn kho với khả năng truy xuất đầy đủ.

### 2. Yêu cầu kiểm soát tồn kho

Dự án nhấn mạnh sự khác biệt giữa tồn kho thực tế, tồn kho dự trữ và tồn kho khả dụng. Tồn kho không chỉ là số lượng, mà còn là khả năng sử dụng đúng và an toàn trong điều kiện thực tế của kho.

### 3. Quản lý lô hàng và ngày hết hạn

Sản phẩm tươi sống cần được quản lý chặt chẽ theo số lô và ngày hết hạn. Dự án áp dụng nguyên tắc FEFO (First Expired, First Out) thay vì chỉ đơn giản là FIFO để ưu tiên hàng có ngày hết hạn gần nhất trong quá trình phân bổ và lấy hàng.

### 4. Khả năng truy xuất và kiểm toán

Mỗi thay đổi về tồn kho phải được ghi lại trong sổ cái tồn kho trung tâm để hệ thống có thể truy vết nguồn gốc, chuyển động và việc sử dụng cuối cùng của từng lô hàng. Điều này giúp kiểm toán và ngăn chặn sai sót trong điều chỉnh tồn kho.

### 5. Quy tắc nghiệp vụ và kiểm soát

Hệ thống sẽ áp dụng nhiều quy tắc thực tế như:

- Tồn kho khả dụng khác với tồn kho thực tế.
- Đơn vị sản phẩm phải được chuyển đổi đúng giữa đơn hàng và đơn vị lưu trữ.
- Khối lượng thực tế (catch weight) có thể khác với số lượng đặt hàng ban đầu.
- Điều chỉnh tồn kho cần có phê duyệt và vẫn phải truy vết được.
- Lịch sử tồn kho là dạng chỉ ghi thêm; thay đổi được thực hiện bằng giao dịch đảo ngược thay vì chỉnh sửa trực tiếp.
- Mọi thay đổi về tồn kho phải đi qua một dịch vụ tồn kho trung tâm.

### 6. Những thách thức cần giải quyết trong quá trình triển khai

Dự án cũng đặt ra nhiều thách thức kỹ thuật và nghiệp vụ quan trọng cần xem xét khi triển khai:

#### Thách thức kỹ thuật

- Duy trì tính nhất quán dữ liệu giữa các giai đoạn nhận hàng, phân bổ, lấy hàng, giao hàng, hoàn trả và điều chỉnh tồn kho.
- Ngăn chặn việc cập nhật tồn kho đồng thời gây ra tình trạng bán quá hoặc tồn kho âm.
- Thiết kế sổ cái tồn kho dạng chỉ ghi thêm, theo đó các giao dịch sai được đảo ngược thay vì chỉnh sửa lịch sử gốc.
- Tính toán đúng các loại tồn kho như thực tế, dự trữ, khả dụng, đã lấy, đã giao và đã hoàn trả cho từng sản phẩm, lô hàng và vị trí lưu trữ.
- Hỗ trợ truy vấn FEFO hiệu quả khi hàng tồn tại ở nhiều vị trí và nhiều lô khác nhau.
- Xử lý chuyển đổi đơn vị và độ chính xác số thập phân khi di chuyển giữa thùng, kilogram và catch weight.
- Đồng bộ hóa giữa API backend và giao diện mobile-first để hướng dẫn nhân viên kho thực hiện đúng quy trình.
- Kiểm thử các kịch bản phức tạp như giao hàng một phần, quy trình phê duyệt và phục hồi sau lỗi.
- Đảm bảo môi trường Docker cục bộ và pipeline GitHub Actions luôn ổn định cho tất cả thành viên trong nhóm.

#### Thách thức về logic hệ thống và nghiệp vụ

- Xác định nguồn dữ liệu duy nhất cho tồn kho bằng cách đảm bảo sổ cái là bản ghi chính thức về mọi di chuyển hàng hóa.
- Tách rõ các trạng thái tồn kho để hàng dự trữ, hàng đã lấy, hàng hỏng và hàng hoàn trả không chồng lấn lên tồn kho khả dụng.
- Áp dụng FEFO đúng theo ngày hết hạn sớm nhất thay vì theo thời điểm nhập hàng.
- Xử lý tình trạng thiếu hàng mức độ nhất quán qua giao hàng một phần, thay thế hoặc đặt hàng tiếp.
- Quản lý sự chênh lệch giữa khối lượng đặt hàng và khối lượng đo thực tế khi sử dụng catch weight.
- Triển khai quy trình phê duyệt cho các điều chỉnh tồn kho và cân bằng stock.
- Xác định quy tắc cho hàng trả lại để chỉ cho nhập lại kho khi đáp ứng tiêu chí hợp lệ.
- Bảo đảm các trạng thái chuyển đổi hợp lệ giữa phiếu nhập, đơn hàng, danh sách lấy hàng, giao hàng và hoàn trả.
- Giữ được tính truy xuất và minh bạch bằng cách lưu đầy đủ thông tin như thời gian, người thực hiện, lý do, chứng từ nguồn, lô hàng, vị trí và số lượng.

## Công nghệ dự kiến

Dự án dự kiến sử dụng:

- Backend: NestJS với TypeScript
- Database: PostgreSQL 16 với Prisma ORM
- Frontend: React + TypeScript trên Next.js
- Môi trường cục bộ: Docker Compose
- CI/CD: GitHub Actions

## Nhận xét

Ngày 01 của tuần 2 giúp tôi hiểu rõ hơn rằng đây không chỉ là một ứng dụng CRUD đơn giản. Đây là một hệ thống vận hành kho cần logic nghiệp vụ chặt chẽ, tính nhất quán dữ liệu và khả năng truy xuất theo kiểu kế toán. Bài học lớn nhất là quản lý tồn kho đối với hàng tươi cần dựa trên các ràng buộc thực tế của kho, không chỉ là đếm số lượng đơn thuần.

## Kết luận

Ngày hôm nay tập trung vào việc hiểu rõ mục đích của dự án và logic vận hành đằng sau nó. Dự án Mini-WMS nhằm tạo ra một mô phỏng kho thực tế và vận hành hiệu quả, nơi quá trình di chuyển hàng, kiểm soát hạn sử dụng, thực hiện đơn hàng và truy xuất dữ liệu đều được xử lý một cách có hệ thống.
