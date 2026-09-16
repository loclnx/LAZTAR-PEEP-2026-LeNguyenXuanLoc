+++
title = "Day 02 - React và Next.js"
weight = 2
+++

### Báo cáo Ngày 02

## A. Lý thuyết

## Phần 1. React cơ bản

### 1. React là gì?

React là thư viện JavaScript mã nguồn mở do Meta phát triển để xây dựng giao diện người dùng, đặc biệt là các ứng dụng web có tính tương tác cao. React tổ chức giao diện thành các component có thể tái sử dụng và cập nhật giao diện dựa trên sự thay đổi của dữ liệu.

### 2. Component trong React là gì? Có mấy loại component?

Component là một phần độc lập của giao diện, có thể nhận dữ liệu đầu vào và trả về giao diện cần hiển thị. Component giúp chia ứng dụng thành những phần nhỏ, dễ phát triển và bảo trì.

React thường có hai dạng component:

- **Function Component:** Được viết bằng hàm JavaScript hoặc TypeScript. Đây là cách được sử dụng phổ biến hiện nay và có thể dùng Hooks.
- **Class Component:** Được viết bằng class, sử dụng các phương thức vòng đời. Cách này chủ yếu xuất hiện trong các dự án React cũ.

### 3. JSX là gì?

JSX là cú pháp mở rộng cho JavaScript, cho phép viết cấu trúc tương tự HTML bên trong mã JavaScript. JSX được biên dịch thành các lời gọi `React.createElement` hoặc cơ chế tương đương trước khi chạy trên trình duyệt.

```jsx
const greeting = <h1>Hello React</h1>;
```

### 4. Props là gì?

Props là dữ liệu được truyền từ component cha xuống component con. Props có tính chỉ đọc, vì component con không nên trực tiếp thay đổi dữ liệu nhận được từ component cha.

```jsx
function Welcome({ name }) {
  return <h1>Xin chào {name}</h1>;
}
```

### 5. State là gì? State khác Props như thế nào?

State là dữ liệu nội bộ của component. Khi state thay đổi, React sẽ yêu cầu component render lại để cập nhật giao diện.

| Đặc điểm                            | Props                              | State                                                           |
| --------------------------------------- | ---------------------------------- | --------------------------------------------------------------- |
| Nguồn dữ liệu                        | Từ component cha                  | Do component tự quản lý                                      |
| Có thể thay đổi trực tiếp không? | Không                             | Không nên thay đổi trực tiếp, phải dùng hàm cập nhật |
| Phạm vi                                | Truyền giữa các component       | Thường thuộc về một component                              |
| Mục đích                             | Cấu hình hoặc truyền dữ liệu | Lưu trạng thái thay đổi của giao diện                    |

### 6. Virtual DOM là gì? Vì sao React sử dụng Virtual DOM?

Virtual DOM là biểu diễn của DOM thật dưới dạng cấu trúc dữ liệu trong bộ nhớ. Khi dữ liệu thay đổi, React tạo Virtual DOM mới, so sánh với phiên bản trước rồi chỉ cập nhật những phần cần thay đổi trên DOM thật.

Cách này giúp giảm số lần thao tác DOM trực tiếp, từ đó làm cho quá trình cập nhật giao diện hiệu quả và dễ kiểm soát hơn.

### 7. Hooks là gì? Kể tên một số Hook phổ biến trong React.

Hooks là các hàm đặc biệt cho phép Function Component sử dụng state, lifecycle và những tính năng khác của React. Một số Hook phổ biến:

- `useState`: Quản lý state.
- `useEffect`: Thực hiện side effect.
- `useContext`: Đọc dữ liệu từ Context.
- `useReducer`: Quản lý state có logic phức tạp.
- `useRef`: Lưu giá trị giữa các lần render hoặc tham chiếu đến phần tử DOM.
- `useMemo`: Ghi nhớ kết quả tính toán.
- `useCallback`: Ghi nhớ một hàm callback.

### 8. `useState` dùng để làm gì?

`useState` dùng để khai báo và cập nhật state trong Function Component.

```jsx
const [count, setCount] = useState(0);

function increase() {
  setCount(count + 1);
}
```

Khi gọi `setCount`, React sẽ render lại component với giá trị mới.

### 9. `useEffect` dùng để làm gì?

`useEffect` dùng để xử lý các side effect, tức là những công việc liên quan đến bên ngoài quá trình render như gọi API, đăng ký sự kiện, cập nhật tiêu đề trang hoặc dùng timer.

```jsx
useEffect(() => {
  document.title = `Count: ${count}`;
}, [count]);
```

Mảng dependency quyết định khi nào effect được chạy lại. Effect cũng có thể trả về một hàm cleanup để hủy timer hoặc sự kiện.

### 10. Lifecycle của một React Component gồm những giai đoạn nào?

Lifecycle của component gồm ba giai đoạn chính:

1. **Mounting:** Component được tạo và đưa vào DOM.
2. **Updating:** Component được cập nhật khi props hoặc state thay đổi.
3. **Unmounting:** Component bị gỡ khỏi DOM.

Với Function Component, `useEffect` thường được dùng để xử lý công việc sau khi render và cleanup khi component bị unmount.

### 11. Client-Side Rendering (CSR) là gì?

CSR là cách render trong đó server gửi HTML cơ bản và JavaScript xuống trình duyệt. Trình duyệt tải JavaScript, chạy ứng dụng và tạo phần lớn giao diện ở phía client.

CSR phù hợp với các ứng dụng tương tác nhiều sau khi ứng dụng đã tải xong, nhưng lần tải đầu tiên có thể chậm và SEO cần được xử lý bổ sung.

### 12. React Router là gì?

React Router là thư viện định tuyến thường dùng trong ứng dụng React. Nó cho phép ánh xạ URL đến component, tạo route lồng nhau, route động, điều hướng bằng code và bảo vệ route.

Ví dụ, URL `/products/10` có thể được ánh xạ đến component hiển thị sản phẩm có mã `10`.

### 13. React thuần có hỗ trợ Routing, SEO và API Server không?

React thuần không tích hợp sẵn Routing, SEO server-side hoặc API Server:

- **Routing:** Có thể dùng thư viện như React Router.
- **SEO:** Có thể dùng thư viện hỗ trợ metadata hoặc triển khai SSR bằng công cụ khác, nhưng React thuần chủ yếu là CSR.
- **API Server:** Cần gọi API từ backend riêng hoặc dùng một dịch vụ backend khác.

Vì vậy, React thuần là thư viện giao diện, không phải một full-stack framework.

### 14. Context API là gì? Khi nào nên sử dụng?

Context API là cơ chế chia sẻ dữ liệu giữa nhiều component mà không cần truyền props qua từng component trung gian.

Context phù hợp với dữ liệu dùng ở nhiều nơi như theme, ngôn ngữ, thông tin người dùng đăng nhập hoặc quyền truy cập. Không nên dùng Context cho mọi state vì khi Context thay đổi, nhiều component sử dụng Context có thể render lại.

### 15. SPA (Single Page Application) là gì?

SPA là ứng dụng web chỉ tải một trang HTML ban đầu. Sau đó JavaScript thay đổi nội dung trên trang và điều hướng giữa các màn hình mà không cần tải lại toàn bộ trang.

SPA cho trải nghiệm chuyển trang nhanh và mượt, nhưng cần chú ý thời gian tải JavaScript ban đầu, SEO và việc quản lý trạng thái trình duyệt.

## Phần 2. So sánh React và Next.js

### 16. Next.js là gì?

Next.js là framework xây dựng ứng dụng web dựa trên React, được phát triển bởi Vercel. Next.js cung cấp sẵn routing, nhiều chiến lược rendering, tối ưu hình ảnh, metadata, API/Route Handlers và các tính năng hỗ trợ deploy.

### 17. Điểm khác biệt cốt lõi giữa React và Next.js là gì?

React là thư viện tập trung vào xây dựng UI. Next.js là framework sử dụng React và cung cấp thêm kiến trúc, quy ước thư mục, routing, server rendering, tối ưu hiệu năng và các tính năng full-stack.

Nói cách khác, React cung cấp nền tảng component, còn Next.js cung cấp một bộ khung hoàn chỉnh để xây dựng ứng dụng web.

### 18. Routing trong React và Next.js khác nhau như thế nào?

Trong React, routing thường phải cài và cấu hình thư viện như React Router, sau đó tự khai báo các route.

Trong Next.js, routing dựa trên cấu trúc file và thư mục. Tùy router được sử dụng, file `app/page.tsx` hoặc `pages/index.tsx` sẽ đại diện cho route `/`. Nhiều trường hợp không cần khai báo route thủ công.

### 19. Rendering trong React và Next.js khác nhau ra sao?

React thuần thường sử dụng CSR: giao diện được tạo chủ yếu trong trình duyệt.

Next.js hỗ trợ nhiều cách rendering:

- CSR.
- SSR: render HTML trên server cho mỗi request.
- SSG: tạo HTML tĩnh trong quá trình build.
- ISR: cập nhật lại trang tĩnh theo chu kỳ hoặc khi được yêu cầu.

### 20. Vì sao Next.js hỗ trợ SEO tốt hơn React thuần?

Với React thuần dùng CSR, HTML ban đầu thường chưa chứa đầy đủ nội dung, nên crawler phải thực thi JavaScript để đọc trang. Next.js có thể tạo HTML chứa nội dung ngay từ server hoặc lúc build.

Next.js cũng hỗ trợ quản lý title, description, Open Graph và các metadata khác. Vì vậy crawler có thể đọc nội dung và metadata sớm hơn, giúp việc lập chỉ mục và chia sẻ liên kết tốt hơn.

### 21. Hiệu năng tải trang đầu tiên của React và Next.js khác nhau như thế nào?

React CSR thường phải tải JavaScript rồi mới tạo giao diện, nên First Load có thể phải chờ lâu hơn.

Next.js có thể gửi HTML đã được render từ server hoặc file HTML tĩnh trước khi JavaScript hoàn tất. Nhờ đó nội dung có thể hiển thị sớm hơn. Tuy nhiên, hiệu năng thực tế còn phụ thuộc vào kích thước bundle, dữ liệu, máy chủ và cách tối ưu của ứng dụng.

### 22. Cấu trúc dự án React và Next.js khác nhau ra sao?

Dự án React thường cho phép tự chọn cấu trúc thư mục và tự cài các thư viện routing, data fetching hoặc cấu hình build.

Next.js có các quy ước rõ ràng hơn, ví dụ:

- `app/` cho App Router.
- `pages/` cho Pages Router.
- `public/` cho tài nguyên tĩnh.
- `next.config.js` hoặc `next.config.ts` cho cấu hình Next.js.
- `layout.tsx`, `page.tsx` và các file đặc biệt cho từng chức năng.

### 23. Next.js có thay thế React không? Vì sao?

Không. Next.js được xây dựng trên React và sử dụng component, JSX, props, state và Hooks của React. Next.js bổ sung các tính năng và quy ước cho ứng dụng lớn hơn, chứ không thay thế kiến thức React.

### 24. Khi nào nên dùng React thuần và khi nào nên dùng Next.js?

Nên dùng React thuần khi:

- Xây dựng SPA nội bộ hoặc dashboard.
- Ứng dụng không yêu cầu SEO cao.
- Muốn tự do lựa chọn thư viện và cấu trúc dự án.
- Backend và frontend được triển khai tách biệt.

Nên dùng Next.js khi:

- Cần SEO và thời gian hiển thị nội dung đầu tiên tốt.
- Xây dựng website công khai, blog, thương mại điện tử hoặc landing page.
- Muốn có file-based routing, SSR, SSG, ISR và tối ưu hình ảnh.
- Muốn kết hợp giao diện với một số chức năng server trong cùng dự án.

## Phần 3. Next.js

### 25. App Router và Pages Router trong Next.js là gì?

**Pages Router** là hệ thống routing truyền thống sử dụng thư mục `pages/`. Nó dùng các hàm như `getStaticProps` và `getServerSideProps` để lấy dữ liệu.

**App Router** là hệ thống routing mới sử dụng thư mục `app/`. App Router hỗ trợ React Server Components, nested layouts, loading UI, error UI và Server Actions. Với dự án mới, App Router thường là lựa chọn được khuyến nghị.

### 26. Server Component và Client Component khác nhau như thế nào?

**Server Component** được render trên server, không gửi toàn bộ logic component xuống trình duyệt. Component này phù hợp để lấy dữ liệu và giảm JavaScript phía client.

**Client Component** chạy ở phía trình duyệt và cần thêm chỉ thị `"use client"`. Nó được dùng khi cần state, event handler, các Hook như `useState`, hoặc API trình duyệt như `window` và `localStorage`.

Server Component giúp giảm bundle client, còn Client Component phù hợp với giao diện có tương tác.

### 27. SSR (Server-Side Rendering) là gì?

SSR là kỹ thuật server tạo HTML cho mỗi request dựa trên dữ liệu hiện tại rồi gửi HTML đó về trình duyệt. SSR phù hợp với nội dung cần cập nhật thường xuyên hoặc phụ thuộc vào request, cookie hay thông tin người dùng.

### 28. SSG (Static Site Generation) là gì?

SSG là kỹ thuật tạo sẵn các file HTML tĩnh trong quá trình build. Khi người dùng truy cập, máy chủ có thể trả về file tĩnh rất nhanh.

SSG phù hợp với nội dung ít thay đổi như blog, tài liệu, trang giới thiệu và landing page.

### 29. ISR (Incremental Static Regeneration) là gì?

ISR là cách cập nhật một trang tĩnh sau khi ứng dụng đã được build mà không cần build lại toàn bộ website. Trang có thể được tạo lại theo thời gian cấu hình hoặc được revalidate khi có sự kiện phù hợp.

ISR kết hợp ưu điểm tốc độ của SSG với khả năng cập nhật nội dung của SSR.

### 30. File-based Routing trong Next.js hoạt động như thế nào?

File-based Routing ánh xạ cấu trúc file và thư mục thành URL:

- `app/page.tsx` tạo route `/`.
- `app/about/page.tsx` tạo route `/about`.
- `app/blog/[slug]/page.tsx` tạo dynamic route `/blog/:slug`.
- `app/blog/layout.tsx` tạo layout dùng chung cho các route bên trong `blog`.

Nhờ đó, route được tạo theo quy ước thay vì phải đăng ký thủ công.

### 31. Dynamic Route trong Next.js là gì?

Dynamic Route là route có một hoặc nhiều phần thay đổi theo URL. Tên thư mục hoặc file được đặt trong dấu ngoặc vuông.

Ví dụ, `app/products/[id]/page.tsx` có thể xử lý các URL `/products/1` và `/products/2`. Giá trị `id` được lấy từ params để tải dữ liệu tương ứng.

### 32. `layout.tsx` trong App Router dùng để làm gì?

`layout.tsx` định nghĩa giao diện dùng chung cho một route và các route con, chẳng hạn như navbar, sidebar hoặc footer. Layout được giữ lại khi chuyển đổi giữa các trang con, giúp tránh việc render lại toàn bộ phần giao diện chung.

Root layout trong `app/layout.tsx` thường chứa thẻ `html`, `body`, font và các provider dùng toàn ứng dụng.

### 33. API Routes (Route Handlers) trong Next.js là gì?

Route Handlers cho phép tạo các endpoint HTTP bên trong thư mục `app`, thường dùng file `route.ts` hoặc `route.js`.

```ts
export async function GET() {
  return Response.json({ message: "Hello" });
}
```

Route Handler có thể xử lý các phương thức như `GET`, `POST`, `PUT`, `PATCH` và `DELETE`, đồng thời dùng để kết nối cơ sở dữ liệu hoặc xử lý logic phía server.

### 34. `getStaticProps` và `getServerSideProps` là gì? Chúng dùng trong trường hợp nào?

Đây là các hàm của **Pages Router**:

- `getStaticProps` lấy dữ liệu tại thời điểm build, phù hợp với SSG. Có thể kết hợp `revalidate` để dùng ISR.
- `getServerSideProps` lấy dữ liệu trên server cho mỗi request, phù hợp với SSR và dữ liệu cần cập nhật liên tục.

Hai hàm này không được dùng trong `app/` của App Router. App Router sử dụng việc fetch dữ liệu trong Server Component và các cơ chế caching/revalidation mới.

### 35. `next/image` giúp tối ưu hình ảnh như thế nào?

`next/image` là component tối ưu hình ảnh của Next.js. Nó hỗ trợ:

- Resize ảnh theo kích thước cần thiết.
- Tự động dùng định dạng hiện đại như WebP hoặc AVIF khi phù hợp.
- Lazy loading ảnh ngoài vùng nhìn thấy.
- Giúp hạn chế layout shift thông qua `width`, `height` hoặc `fill`.
- Tối ưu ảnh local và ảnh từ remote domain đã được khai báo.

Khi dùng ảnh từ nguồn bên ngoài, cần khai báo domain hoặc `remotePatterns` trong `next.config.js`/`next.config.ts`.

### 36. Middleware trong Next.js là gì?

Middleware là đoạn code chạy trước khi request được hoàn tất. Middleware có thể kiểm tra đường dẫn, xác thực người dùng, redirect, rewrite URL, thêm hoặc đọc cookie và header.

Middleware thường được đặt trong file `middleware.ts` ở thư mục gốc dự án. Cần cấu hình `matcher` nếu chỉ muốn áp dụng middleware cho một số route.

### 37. Làm thế nào để điều hướng giữa các trang trong Next.js?

Có thể điều hướng bằng nhiều cách:

- Dùng component `Link` cho liên kết trong ứng dụng:

```tsx
<Link href="/about">About</Link>
```

- Dùng `useRouter` trong Client Component cho điều hướng bằng code:

```tsx
const router = useRouter();
router.push('/about');
```

- Dùng `redirect` trong Server Component hoặc server-side logic khi cần chuyển hướng từ phía server.

### 38. Metadata và SEO trong Next.js được xử lý như thế nào?

Next.js hỗ trợ khai báo metadata tĩnh bằng `metadata` hoặc tạo metadata động bằng `generateMetadata` trong App Router.

Metadata có thể gồm `title`, `description`, `keywords`, ảnh Open Graph, Twitter Card và canonical URL. Nội dung trang cần có heading hợp lý, ảnh có `alt`, liên kết rõ ràng và HTML có ngữ nghĩa để hỗ trợ SEO tốt hơn.

### 39. Next.js có hỗ trợ TypeScript không?

Có. Next.js hỗ trợ TypeScript trực tiếp. Khi tạo hoặc đổi file sang `.ts` và `.tsx`, Next.js có thể tạo `tsconfig.json` và cài các kiểu cần thiết. TypeScript giúp kiểm tra kiểu dữ liệu cho props, state, API response và các hàm trong quá trình phát triển.

### 40. Có thể deploy dự án Next.js lên những nền tảng nào?

Có thể deploy Next.js lên nhiều nền tảng như:

- **Vercel:** Nền tảng được tối ưu cho Next.js, hỗ trợ preview deployment và CI/CD từ Git.
- **Netlify:** Phù hợp với nhiều ứng dụng frontend và có hỗ trợ Next.js.
- **AWS:** Có thể triển khai thông qua Amplify, server riêng, container hoặc các dịch vụ AWS khác.
- **Docker:** Đóng gói ứng dụng để chạy trên bất kỳ máy chủ nào hỗ trợ container.
- **Railway, Render, DigitalOcean** và các nền tảng Node.js khác.

Khi deploy cần kiểm tra biến môi trường, lệnh build, cấu hình image remote, API và chế độ rendering mà ứng dụng đang sử dụng.

## Khó khăn gặp phải

- **Thiết kế wireframe:** Khó khăn là sắp xếp bố cục wireframe sao cho đúng với yêu cầu, thể hiện rõ user flow và dễ đọc. Cần xác định thứ tự ưu tiên của nội dung, vị trí các chức năng chính và giữ bố cục nhất quán trước khi bắt đầu code.
- **Lựa chọn React hay Next.js cho SEO:** Khi muốn xây dựng website có SEO, Next.js phù hợp hơn React thuần vì hỗ trợ SSR và SSG, giúp nội dung được render thành HTML sớm hơn để crawler dễ đọc. Next.js cũng có sẵn cách quản lý metadata như title, description và Open Graph. React vẫn có thể làm SEO, nhưng thường cần bổ sung thêm thư viện hoặc giải pháp rendering khác nên phải tự cấu hình nhiều hơn.

## Landing Page

[Xem portfolio cá nhân](https://loclnxportfolio.vercel.app/)
