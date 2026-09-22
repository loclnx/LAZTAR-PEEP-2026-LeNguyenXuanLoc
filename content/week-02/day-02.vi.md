+++
title = "Ngày 02 - 22/09/2026 (Nghiên cứu Database và ORM)"
weight = 2
+++

## Công việc đã làm

- Tìm hiểu công nghệ database được sử dụng trong dự án: PostgreSQL 16.
- Nghiên cứu Prisma ORM và vai trò của nó trong backend NestJS.
- Xác định cách hai công nghệ này phối hợp với nhau trong hệ thống Mini-WMS.
- Phân tích tầm quan trọng của chúng đối với dữ liệu tồn kho, giao dịch và tính truy xuất.

## Tổng quan

Dự án Mini-WMS sẽ sử dụng PostgreSQL 16 làm cơ sở dữ liệu quan hệ và Prisma ORM làm lớp truy cập dữ liệu cho backend. Đây là sự kết hợp phù hợp cho một hệ thống kho hàng vì dự án liên quan đến nhiều mối quan hệ phức tạp, giao dịch tồn kho, lịch sử kiểm toán và tính nhất quán của dữ liệu.

Database là nền tảng của hệ thống. Nó lưu trữ mọi dữ liệu như người dùng, sản phẩm, lô hàng, số lượng tồn kho, đơn hàng, giao hàng và sổ cái tồn kho. Prisma sau đó giúp backend NestJS tương tác với PostgreSQL theo cách an toàn và rõ ràng hơn bằng TypeScript.

## PostgreSQL 16

PostgreSQL là một hệ quản trị cơ sở dữ liệu quan hệ mạnh mẽ và mã nguồn mở. Trong dự án này, nó sẽ là nguồn dữ liệu vững chắc cho tất cả hoạt động kho.

### PostgreSQL sẽ lưu những gì

PostgreSQL sẽ lưu trữ các dữ liệu quan trọng như:

- Người dùng, vai trò và quyền truy cập.
- Kho hàng và vị trí lưu trữ.
- Sản phẩm, SKU, đơn vị tính và quy đổi đơn vị.
- Lô hàng tồn kho và ngày hết hạn.
- Sổ cái tồn kho cho mọi biến động.
- Phiếu nhập, hoạt động putaway, kiểm kê và điều chỉnh.
- Đơn hàng, phân bổ, phiếu lấy hàng, đóng gói, giao hàng và hoàn trả.
- Hồ sơ phê duyệt và dữ liệu kiểm toán.

### Vì sao PostgreSQL phù hợp

#### 1. Mô hình dữ liệu quan hệ

Dữ liệu kho thường chứa nhiều mối quan hệ. Ví dụ, một sản phẩm có thể có nhiều lô hàng, một kho có nhiều vị trí lưu trữ, và một đơn hàng có nhiều dòng đơn. PostgreSQL hỗ trợ tốt các cấu trúc này và duy trì tính toàn vẹn dữ liệu.

#### 2. Tính toàn vẹn dữ liệu

PostgreSQL cung cấp khóa chính, khóa ngoại, ràng buộc duy nhất, ràng buộc kiểm tra và giao dịch. Điều này giúp ngăn chặn các bản ghi không hợp lệ hoặc không đầy đủ.

#### 3. Hỗ trợ giao dịch

Một hoạt động trong kho thường cập nhật nhiều bảng cùng lúc. Ví dụ, xác nhận phiếu nhập có thể tạo bản ghi nhập, bản ghi lô hàng, biến động tồn kho và sổ cái trong một giao dịch. PostgreSQL có thể commit tất cả đồng thời hoặc rollback nếu xảy ra lỗi.

#### 4. Kiểm soát đồng thời

Hệ thống có thể có nhiều người dùng làm việc cùng lúc. PostgreSQL cung cấp row-level locking và transaction isolation, giúp giảm xung đột khi phân bổ hoặc đặt chỗ hàng tồn kho.

#### 5. Độ chính xác số học

Dự án làm việc với số lượng, trọng lượng và hệ số quy đổi. Kiểu NUMERIC của PostgreSQL đáng tin cậy hơn số thực vì nó giữ nguyên độ chính xác và tránh sai số trong tính toán tồn kho.

#### 6. Khả năng truy vấn

Báo cáo kho và truy vấn FEFO cần lọc hàng theo sản phẩm, lô, vị trí, ngày hết hạn và trạng thái. SQL rất phù hợp cho các truy vấn này và hỗ trợ tổng hợp dữ liệu hiệu quả.

## Prisma ORM

Prisma ORM là một công cụ database thân thiện với TypeScript, giúp kết nối backend NestJS với PostgreSQL. Nó cho phép lập trình viên làm việc với dữ liệu dưới dạng kiểu dữ liệu mạnh và rõ ràng.

### Prisma sẽ làm gì

- Định nghĩa các model, trường, enum, index và mối quan hệ trong Prisma schema.
- Tạo các type TypeScript và Prisma Client.
- Đọc và ghi dữ liệu bằng các truy vấn kiểu an toàn.
- Tạo và áp dụng migration database.
- Hỗ trợ các hoạt động database trong giao dịch.
- Cung cấp lớp truy cập dữ liệu rõ ràng cho ứng dụng.

### Vì sao Prisma phù hợp

#### 1. Type safety

Prisma sinh ra các type TypeScript từ schema, giúp tránh lỗi khi chọn trường hoặc tạo dữ liệu. Điều này giảm nhiều lỗi phổ biến trong quá trình phát triển.

#### 2. Truy cập dữ liệu rõ ràng

Các truy vấn Prisma dễ đọc hơn so với SQL truyền thống trong nhiều thao tác CRUD. Điều này cải thiện năng suất nhóm và làm cho code dễ bảo trì hơn.

#### 3. Quản lý migration

Prisma Migrate lưu các thay đổi schema dưới dạng migration có phiên bản. Điều này giúp mọi nhà phát triển và pipeline CI giữ được cấu trúc database đồng bộ qua nhiều môi trường.

#### 4. Xử lý mối quan hệ

Prisma dễ dàng làm việc với dữ liệu liên quan, ví dụ như lấy một đơn hàng cùng các dòng, phân bổ và lô hàng trong một luồng xử lý.

#### 5. Năng suất phát triển

Prisma giảm mã lặp lại và cho phép nhà phát triển backend tập trung nhiều hơn vào logic nghiệp vụ thay vì chi tiết SQL thấp cấp.

## Cách PostgreSQL và Prisma hoạt động cùng nhau

Prisma schema mô tả cấu trúc database. Khi schema thay đổi, Prisma Migrate tạo các file migration để cập nhật PostgreSQL. Sau đó backend NestJS sử dụng Prisma Client để đọc và ghi dữ liệu.

```text
NestJS Service
      |
      v
Prisma Client / Prisma ORM
      |
      v
PostgreSQL 16 Database
```

Ví dụ, khi nhân viên kho xác nhận phiếu nhập, một giao dịch Prisma có thể:

1. Tạo bản ghi goods receipt.
2. Thêm các dòng phiếu nhập.
3. Tạo bản ghi lô hàng khi cần.
4. Tạo các giao dịch sổ cái cho số lượng nhập.
5. Cập nhật số lượng tồn kho hiện tại.

Nếu bất kỳ bước nào thất bại, PostgreSQL có thể rollback toàn bộ thay đổi trong cùng một giao dịch. Điều này giúp database luôn nhất quán.

## Tầm quan trọng đối với dự án

Dự án phụ thuộc rất nhiều vào logic tồn kho chính xác, vì vậy thiết kế database là một phần cực kỳ quan trọng. PostgreSQL và Prisma tạo ra nền tảng phù hợp để xử lý:

- Tính toán tồn kho
- Theo dõi lô hàng và ngày hết hạn
- Tính toàn vẹn của sổ cái tồn kho
- Logic phân bổ và đặt chỗ đơn hàng
- Quy trình phê duyệt và kiểm toán
- Tính an toàn của giao dịch

### Một số lưu ý thiết kế quan trọng

- Sử dụng kiểu NUMERIC cho số lượng, trọng lượng và hệ số quy đổi.
- Giữ sổ cái tồn kho dạng chỉ ghi thêm.
- Xác định rõ khóa ngoại và ràng buộc.
- Thêm index cho các truy vấn thường gặp theo sản phẩm, kho, lô, ngày hết hạn và thời gian ledger.
- Bọc mọi thao tác thay đổi tồn kho trong transaction.
- Dùng locking hoặc kiểm tra đồng bộ hóa để tránh bán quá số lượng.
- Review Prisma migration trong pull request và giữ chúng dưới dạng phiên bản.

## Nhận xét

Ngày 02 giúp tôi hiểu rõ hơn vì sao công nghệ database là một trong những phần quan trọng nhất của hệ thống kho. Một Mini-WMS không chỉ là giao diện và quy trình nghiệp vụ, mà còn là cách lưu trữ dữ liệu đúng và đảm bảo thông tin tồn kho vẫn đáng tin cậy khi nhiều thao tác xảy ra đồng thời.

PostgreSQL cung cấp nền tảng database mạnh mẽ về giao dịch, trong khi Prisma giúp code backend dễ viết hơn, an toàn hơn và dễ bảo trì hơn. Cả hai cùng tạo thành một sự kết hợp thực tế và chuyên nghiệp cho dự án.

## Kết luận

Nghiên cứu này cho thấy PostgreSQL 16 và Prisma ORM là hai công nghệ quan trọng để xây dựng một Mini-WMS đáng tin cậy. PostgreSQL đảm bảo lưu trữ dữ liệu mạnh mẽ và an toàn giao dịch, còn Prisma mang đến cách làm việc hiện đại, kiểu dữ liệu chặt chẽ và dễ bảo trì từ backend NestJS. Sự kết hợp này sẽ hỗ trợ việc kiểm soát tồn kho chính xác, truy xuất nguồn gốc và phát triển phần mềm bền vững trong suốt dự án.
