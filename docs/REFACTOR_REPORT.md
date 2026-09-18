# Landing Page Refactor Report

## Mục tiêu

Refactor Landing Page Next.js + TypeScript từ một `app/page.tsx` lớn thành cấu trúc component-based, dễ bảo trì và mở rộng, đồng thời giữ nguyên giao diện, nội dung và chức năng hiện có.

## Audit ban đầu

### Cấu trúc trước refactor

- `app/page.tsx` chứa toàn bộ JSX cho landing page, state đổi ngôn ngữ và CSS inline của language switcher.
- `layout/SiteHeader.tsx` là component client cho navigation và language switcher.
- `data/landing-mock-data.tsx` chứa toàn bộ nội dung song ngữ Anh/Việt.
- `app/globals.css` chứa toàn bộ styling và responsive rules.

### Sections đã xác định

1. Header / Navigation
2. Hero
3. Overview
4. Capabilities
5. Use case
6. Guide / Onboarding
7. Safety
8. Comparison
9. Footer

### Logic và client boundary

- Logic hiện có chỉ là state `language` (`en` / `vi`) để đổi toàn bộ copy trên trang.
- `LandingPage` là client component vì state ngôn ngữ cập nhật nội dung của nhiều section.
- `SiteHeader` là client component vì chứa event handler của các nút đổi ngôn ngữ.
- Các section không có state, effect hoặc browser API riêng.

## Cấu trúc sau refactor

```text
app/
├── globals.css
├── layout.tsx
└── page.tsx

components/
├── landing/
│   └── LandingPage.tsx
├── layout/
│   ├── SiteFooter.tsx
│   └── SiteHeader.tsx
├── sections/
│   ├── CapabilitiesSection.tsx
│   ├── ComparisonSection.tsx
│   ├── GuideSection.tsx
│   ├── HeroSection.tsx
│   ├── OverviewSection.tsx
│   ├── SafetySection.tsx
│   └── UseCaseSection.tsx
└── ui/
    └── ArrowIcon.tsx

data/
└── landing-data.tsx

types/
└── landing.ts
```

## Mapping từ `app/page.tsx` cũ

| Phần cũ | File mới |
| --- | --- |
| Header | `components/layout/SiteHeader.tsx` |
| Hero | `components/sections/HeroSection.tsx` |
| Overview | `components/sections/OverviewSection.tsx` |
| Capabilities | `components/sections/CapabilitiesSection.tsx` |
| Use case | `components/sections/UseCaseSection.tsx` |
| Guide | `components/sections/GuideSection.tsx` |
| Safety | `components/sections/SafetySection.tsx` |
| Comparison | `components/sections/ComparisonSection.tsx` |
| Footer | `components/layout/SiteFooter.tsx` |
| Arrow glyph lặp lại | `components/ui/ArrowIcon.tsx` |
| Language state và composition | `components/landing/LandingPage.tsx` |

## Những thay đổi đã thực hiện

- Rút gọn `app/page.tsx` thành Server Component chỉ render `LandingPage`.
- Tách từng section thành component có một trách nhiệm rõ ràng.
- Di chuyển language state vào `components/landing/LandingPage.tsx`.
- Di chuyển header từ `layout/` sang `components/layout/` để thống nhất vị trí component UI.
- Tách reusable arrow glyph thành `ArrowIcon`.
- Đổi tên `landing-mock-data.tsx` thành `landing-data.tsx`; dữ liệu thực tế được giữ nguyên, không tạo mock data mới.
- Thêm `LandingContent`, `ContentCard`, `ComparisonRow` và `Language` để type-safe static data và props.
- Chuyển CSS inline của language switcher vào `app/globals.css`, không thay đổi rule CSS hay giao diện.

## Những gì được giữ nguyên

- Nội dung tiếng Anh và tiếng Việt.
- Default language là tiếng Anh.
- Navigation anchor, smooth scrolling và các `id` section.
- Hành vi đổi ngôn ngữ.
- Semantic HTML hiện có.
- Toàn bộ class name, CSS, breakpoint, spacing, màu sắc và responsive behavior.
- Metadata, dependencies, Next.js config, Tailwind setup và business logic.

## Validation

| Kiểm tra | Kết quả |
| --- | --- |
| `npm run lint` | Passed |
| `npm run build` | Passed |
| TypeScript trong production build | Passed |
| `git diff --check` | Passed |

## Ghi chú phát triển tiếp theo

- Có thể bổ sung test UI hoặc browser test nếu dự án phát triển thêm interactive behavior.
- Nếu nội dung được lấy từ CMS/API trong tương lai, có thể thay `data/landing-data.tsx` bằng data access layer mà không cần thay đổi các section.
- Khi thêm route nội bộ, ưu tiên `next/link`; landing hiện tại chỉ dùng hash anchor trong cùng trang nên giữ thẻ `<a>` là phù hợp.
