# Hướng dẫn thay đổi host CDN cho ứng dụng Frontend sau khi build

Tài liệu này hướng dẫn cách cấu hình file `index.html` (sau khi đã build ra bản production) để trình duyệt ưu tiên tải các file tĩnh nặng (như JS, CSS) từ một máy chủ CDN thay vì tải trực tiếp từ server gốc. Việc này giúp giảm tải cho server gốc và tăng tốc độ tải trang đáng kể.

## 1. File cần can thiệp

- **Tên file:** `index.html` (Nằm trong thư mục `dist/` hoặc `build/` sinh ra sau khi chạy lệnh build frontend, ví dụ: `npm run build`).
- **Thời điểm thực hiện:** 
  - **SAU KHI** tiến trình build production hoàn tất.
  - **TRƯỚC KHI** đem thư mục build này deploy lên máy chủ gốc.
- **Tại sao lại sửa file này?** File `index.html` là cánh cửa đầu tiên trình duyệt đọc. Trong này chứa các đường dẫn gọi đến các file script và style. Ta cần đổi các đường dẫn này trỏ về CDN.

## 2. Nhận diện các file Bundle cần đưa lên CDN

Khi build Frontend (Angular, React, Vue...), công cụ build sẽ đóng gói code của bạn thành các file "bundle" (JS/CSS). 

**Đặc điểm của các file này:**

- **Dung lượng lớn:** Thường chiếm phần lớn thời gian tải trang.
- **Tên file có chứa mã hash:** Ví dụ `main.fc102ec376494d1a.js`. Mã hash (`fc102...`) sẽ **thay đổi mỗi khi code bên trong bị thay đổi và build lại**. Điều này giúp trình duyệt tự động cập nhật cache.

**Các file ưu tiên đưa lên CDN bao gồm:**

| Loại file | Tiền tố thường gặp (Angular) | Mục đích |
|---|---|---|
| **Script (.js)** | `polyfills.*.js` | Code hỗ trợ các trình duyệt cũ. |
| **Script (.js)** | `main.*.js` | Chứa logic chính của toàn bộ ứng dụng. |
| **Script (.js)** | `scripts.*.js` | Các thư viện JS bên thứ 3 (nếu có). |
| **Style (.css)** | `styles.*.css` | Chứa toàn bộ CSS chung của ứng dụng. |

> **Lưu ý:** Không cần thuộc lòng tên hash. Bạn chỉ cần tìm các thẻ gọi file có đuôi `.js` và `.css` trong file `index.html`.

## 3. Nhận diện các thẻ cần sửa trong `index.html`

Khi mở file `index.html` vừa build xong, bạn sẽ thấy các đoạn code gọi file tĩnh bằng **đường dẫn tương đối** (chỉ có tên file). 

### 3.1. Thẻ cần thay đổi đường dẫn

- **Thẻ gọi Javascript (`<script>`):** Sửa thuộc tính `src`. Ví dụ: `<script src="main.fc102ec376494d1a.js"></script>`
- **Thẻ gọi CSS (`<link>`):** Sửa thuộc tính `href`. Ví dụ: `<link rel="stylesheet" href="styles.34474810a2b8269b.css">`

### 3.2. Thẻ TUYỆT ĐỐI KHÔNG thay đổi

- **`<base href="...">`**: Thẻ này dùng để định tuyến (routing) nội bộ của ứng dụng Frontend. Nếu bạn đổi nó thành URL của CDN, toàn bộ router chuyển trang của ứng dụng sẽ bị hỏng. Hãy giữ nguyên (thường là `<base href="/">`).


## 4. Quy trình thay đổi Host sang CDN

Giả sử ứng dụng web của bạn chạy tại `https://hrmcore.prod.com` và bạn muốn tải file từ CDN có địa chỉ là `https://cdn.example.com/`.

### 4.1. Đưa file lên máy chủ CDN

Bạn copy toàn bộ các file `.js`, `.css` (và thư mục assets/images nếu cần) từ thư mục build lên máy chủ CDN. **Bắt buộc phải giữ nguyên cấu trúc thư mục** so với bản build gốc.

### 4.2. Thay thế đường dẫn trong `index.html`

Mở file `index.html` và chèn thêm URL của CDN (`https://cdn.example.com/`) vào ngay trước tên file JS/CSS.

### 5. Cấu hình CORS (Cross-Origin Resource Sharing)

#### 5.1. Tại sao cần cấu hình CORS?

Khi bạn tách riêng file `index.html` và các bundle (JS, CSS) ra hai nơi khác nhau, trình duyệt sẽ nhận diện đây là hai nguồn (origin) hoàn toàn độc lập:

- **Origin (Nơi chạy website):** `https://hrmcore.prod.com`
- **Resource/CDN (Nơi chứa file tĩnh):** `https://hrm-core.vnrlocal.com`

Theo cơ chế bảo mật mặc định (Same-Origin Policy), trình duyệt sẽ **chặn (block)** website tải và thực thi các file script/style từ một domain khác. Để khắc phục, ta cần cấu hình Nginx trên máy chủ chứa file (`https://hrm-core.vnrlocal.com`) trả về các header CORS hợp lệ để cấp quyền truy cập cho domain chạy web (`https://hrmcore.prod.com`).

#### 5.2. Cấu hình Nginx trên máy chủ chứa file (hrm-core.vnrlocal.com)

Mở file cấu hình Nginx của domain `hrm-core.vnrlocal.com` (ví dụ: `default.conf`) và thêm cấu hình CORS vào block `location` xử lý các file tĩnh (`.js`, `.mjs`, `.css`,...):

```nginx
    # Cấu hình cấp quyền CORS cho các file tĩnh (js, css, font, image...)
    location ~* \.(js|mjs|css|png|jpg|jpeg|gif|ico|svg|woff|woff2|ttf|eot)$ {
        # (Tuỳ chọn) Bật cache cho file tĩnh để tăng tốc độ tải
        expires 1y;
        add_header Cache-Control "public, immutable";

        # Thêm CORS Headers cấp quyền cho hrmcore.prod.com
        add_header 'Access-Control-Allow-Origin' 'https://hrmcore.prod.com' always;
        add_header 'Access-Control-Allow-Methods' 'GET, OPTIONS' always;
        add_header 'Access-Control-Allow-Headers' 'Origin, Content-Type, Accept' always;

        # Xử lý preflight request (tuỳ chọn dự phòng cho các request phức tạp)
        if ( $request_method = 'OPTIONS') {
            add_header 'Access-Control-Allow-Origin' 'https://hrmcore.prod.com' always;
            add_header 'Access-Control-Allow-Methods' 'GET, OPTIONS' always;
            add_header 'Access-Control-Allow-Headers' 'Origin, Content-Type, Accept' always;
            add_header 'Access-Control-Max-Age' 3600;
            add_header 'Content-Length' 0;
            return 204;
        }
    }

## 6. Lưu ý

- **CORS:** CDN phải cho phép trình duyệt tải script/style từ origin của trang (header phù hợp nếu dùng credential/module).