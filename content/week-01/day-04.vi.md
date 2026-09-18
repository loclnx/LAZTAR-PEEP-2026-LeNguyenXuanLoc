+++
title = "Day 04 - 18/09/2026 (REMOTE) Refactor Landing Page"
weight = 4
+++

### Báo cáo ngày 04

## A. Công việc đã hoàn thành

- Refactor landing page từ một file lớn thành cấu trúc component-based theo hướng dễ bảo trì hơn.
- Tách trang thành các component riêng biệt cho header, hero, overview, capabilities, use case, guide, safety, comparison và footer.
- Chuyển logic trạng thái ngôn ngữ và việc kết hợp nội dung sang file `components/landing/LandingPage.tsx`.
- Di chuyển CSS của language switcher từ trong component ra `app/globals.css` mà không làm thay đổi giao diện hiện có.
- Tách icon mũi tên lặp lại sang `components/ui/ArrowIcon.tsx` để tái sử dụng.
- Đổi tên file dữ liệu từ `landing-mock-data.tsx` thành `landing-data.tsx` đồng thời giữ nguyên nội dung song ngữ thực tế.
- Thêm các type rõ ràng như `LandingContent`, `ContentCard`, `ComparisonRow` và `Language` để tăng tính an toàn và dễ quản lý dữ liệu.
- Giữ nguyên ngôn ngữ mặc định là tiếng Anh và bảo toàn anchor navigation, smooth scrolling và các section ID.
- Kiểm tra chất lượng dự án bằng lệnh `npm run lint` và `npm run build`, cả hai đều thành công.

## B. Thách thức gặp phải

### 1. Chia một landing page lớn thành các phần rõ ràng hơn

Trang ban đầu chứa toàn bộ JSX, nội dung song ngữ và logic state trong cùng một file, khiến việc đọc, bảo trì và mở rộng trở nên khó khăn.

Giải pháp là tách nội dung thành từng section riêng, mỗi section chịu trách nhiệm cho một phần UI, còn landing page chỉ đóng vai trò kết hợp các phần đó lại.

### 2. Giữ nguyên hành vi cũ trong khi cải tiến cấu trúc

Trang có chức năng đổi ngôn ngữ, điều hướng bằng anchor và layout cố định. Việc refactor có thể làm hỏng các chức năng tương tác hoặc phá vỡ giao diện.

Để tránh điều này, nhóm giữ nguyên nội dung, ID của từng section, ngôn ngữ mặc định và rule CSS, chỉ tách logic và cấu trúc ra thành component cấp cao hơn.

### 3. Quản lý dữ liệu song ngữ sạch hơn và an toàn hơn

Landing page chứa rất nhiều nội dung tiếng Anh và tiếng Việt. Nếu để trực tiếp trong component, việc cập nhật hoặc sửa đổi sẽ rất dễ sai và mất thời gian.

Giải pháp là tập trung dữ liệu vào `data/landing-data.tsx` và định nghĩa các type TypeScript cho cấu trúc nội dung. Điều này giúp cập nhật bản dịch hoặc bổ sung section dễ dàng hơn mà không ảnh hưởng đến layout.

### 4. Bảo toàn responsive trong quá trình refactor

Vì trang sử dụng nhiều class, khoảng cách và breakpoint khác nhau, nên khi tách component có thể dễ dẫn đến thay đổi layout hoặc sai bố cục trên màn hình nhỏ.

Trong quá trình refactor, tất cả class name, breakpoint và rule responsive đều được giữ nguyên, nên giao diện cuối cùng vẫn giữ được vẻ ngoài tương đồng với phiên bản gốc.

## C. Kết quả

Landing page hiện đã trở nên dễ bảo trì, dễ mở rộng và dễ phát triển hơn, đồng thời vẫn giữ nguyên giao diện, trải nghiệm song ngữ và chất lượng production-ready.
