# Thay đổi host CDN trong `index.html` sau khi build

Hướng dẫn chỉnh **bản `index.html` đã build** để các bundle **JS/CSS** tải từ CDN.

---

## 1. File cần sửa

- `.../index.html`.

Sửa **sau** khi build production xong, **trước** khi upload `index.html` lên server gốc (hoặc trong bước CI/CD).

---

## 2. Đề xuất các File có dung lượng lớn


**Lưu ý:** Phần hash trong tên file  **đổi mỗi lần build**. Sau mỗi build mới, mở lại Network và đối chiếu pattern `polyfills.*.js`, `scripts.*.js`, `main.*.js`, v.v.

| Tên file (ví dụ một build) | Loại |
|----------------------------|------|
| `polyfills.13ea46dd1e2a0a82.js` | script |
| `main.fc102ec376494d1a.js` | script | 
| `scripts.254c65203634a1fd.js` | script |
| `styles.34474810a2b8269b.css` | stylesheet |

**Ưu tiên CDN trong `index.html` sau build:** đổi host cho đủ các thẻ trỏ tới `polyfills*.js`, `styles*.css`.

---

## 3. Các thẻ trong `index.html` sau build cần đổi host

| Thẻ | Thuộc tính | Nội dung điển hình |
|-----|------------|-------------------|
| `<script` | `src` | `runtime*.js`, `polyfills*.js`, `main*.js`, `scripts*.js`|
| `<link rel="stylesheet"` | `href` | `styles*.css` |

**`<base href="...">`:** thường **giữ** là đường dẫn app trên server, **không** thay bằng URL CDN — trừ khi team đã thống nhất kiến trúc đặc biệt (ảnh hưởng router và tải JSON/config theo `base`).

---

## 4. Cách thay đổi host

### 4.1. Nguyên tắc

1. Upload các file bundle **`.js`** và **`.css`** đã build lên CDN, **giữ nguyên cấu trúc đường dẫn tương đối** so với root deploy..
2. Trong `index.html` đã build, chỉ đổi `src`/`href` của **script và stylesheet bundle** (mục 3) từ origin gốc sang **URL đầy đủ CDN**.

**Ví dụ** — CDN base: `https://cdn.example.com/`

- `src="runtime.abc123.js"` → `src="https://cdn.example.com/runtime.abc123.js"`
- `href="styles.def456.css"` → `href="https://cdn.example.com/styles.def456.css"`

Đường dẫn sau `https://cdn.example.com/...` phải **khớp** vị trí thật của file trên CDN.

### 4.2. Thủ công

Mở `.../index.html`, chỉ sửa các `<script src="...js">` và `<link rel="stylesheet" href="...css">` của bundle.

---

## 5. Lưu ý

- **CORS:** CDN phải cho phép trình duyệt tải script/style từ origin của trang (header phù hợp nếu dùng credential/module).
