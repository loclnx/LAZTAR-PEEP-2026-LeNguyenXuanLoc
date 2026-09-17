+++
title = "Day 03 - 17/09/2026 (REMOTE) Landing Page GPT-6 Astra"
weight = 3
+++

### Báo cáo Ngày 03

## A. Công việc đã thực hiện

- Xây dựng landing page giới thiệu về GPT-6 Astra bằng Next.js.
- Thiết kế giao diện theo hướng hiện đại, tập trung vào chủ đề AI agent với tông xanh lá, nền sáng và bố cục responsive.
- Hoàn thiện các phần nội dung chính:
  - GPT-6 Astra là gì.
  - Khả năng và sức mạnh của GPT-6 Astra.
  - Quy trình đăng ký và sử dụng gồm 4 bước.
  - Lưu ý về an toàn khi sử dụng.
  - Bảng so sánh GPT-6 Astra và GPT-5.6 Sol.
- Thêm thanh điều hướng bằng anchor link để di chuyển nhanh đến từng phần nội dung.
- Thêm hỗ trợ song ngữ:
  - Tiếng Anh là ngôn ngữ mặc định.
  - Người dùng có thể chuyển đổi giữa EN và VI trực tiếp trên thanh điều hướng.
- Tách toàn bộ mock content song ngữ khỏi giao diện và đặt tại:
  - `data/landing-mock-data.tsx`
- Cập nhật metadata trang sang tiếng Việt, gồm tiêu đề và mô tả cho GPT-6 Astra.
- Kiểm tra production build thành công bằng lệnh `npm run build`.

## B. Khó khăn gặp phải

### 1. Tổ chức nội dung dài trên một landing page

Landing page cần bao gồm nhiều nhóm nội dung như giới thiệu, năng lực, hướng dẫn, an toàn và so sánh. Thách thức là giữ được mạch đọc rõ ràng, tránh biến giao diện thành một bài viết dài và khó theo dõi.

Giải pháp là chia nội dung thành các section có nhận diện riêng, dùng số thứ tự, tiêu đề lớn, màu nền khác nhau và bảng so sánh để người đọc dễ quét thông tin.

### 2. Hỗ trợ song ngữ nhưng vẫn giữ giao diện ổn định

Nội dung tiếng Anh và tiếng Việt có độ dài khác nhau. Điều này dễ làm các card, tiêu đề hoặc bảng so sánh bị lệch bố cục, đặc biệt trên màn hình nhỏ.

Giải pháp là sử dụng layout linh hoạt bằng CSS Grid, font size responsive với `clamp()`, và breakpoint cho tablet/mobile.

### 3. Tách mock data khỏi component

Ban đầu nội dung song ngữ được đặt trực tiếp trong `page.tsx`, khiến component dài và khó bảo trì. Khi thêm nội dung hoặc chỉnh sửa bản dịch, việc tìm đúng vị trí trở nên mất thời gian.

Giải pháp là chuyển toàn bộ dữ liệu hiển thị sang file `data/landing-mock-data.tsx`. Component trang hiện chỉ phụ trách hiển thị UI và xử lý trạng thái đổi ngôn ngữ.

### 4. Biểu diễn tiêu đề có định dạng trong mock data

Một số tiêu đề cần xuống dòng hoặc nhấn mạnh bằng chữ nghiêng. Khi tách sang mock data, các giá trị này cần được biểu diễn dưới dạng JSX để giao diện giữ nguyên cách trình bày.

Vì vậy mock data được đặt trong file `.tsx` thay vì `.ts`, giúp dữ liệu có thể chứa các phần tử React khi thật sự cần thiết.

## C. Kết quả

Landing page hiện đã có nội dung đầy đủ, hỗ trợ EN/VI, responsive trên nhiều kích thước màn hình và có cấu trúc data/UI tách biệt để thuận tiện mở rộng sau này.


[gpt6astra-pi.vercel.app](https://gpt6astra-pi.vercel.app/)
