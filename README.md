[README.md](https://github.com/user-attachments/files/32409134/README.md)
# Danh bạ Khu phố - UBND Phường Mỹ Ngãi

Ứng dụng web tĩnh, mobile-first, chạy trực tiếp trên Vercel/Netlify, không cần backend hoặc database.

## Cấu trúc

- `index.html`: giao diện chính
- `style.css`: giao diện responsive, light/dark mode
- `data.js`: toàn bộ dữ liệu 12 khu phố
- `app.js`: render, tìm kiếm không dấu, bộ lọc, gọi điện, sao chép, chia sẻ, dark mode
- `manifest.json`: cấu hình PWA
- `service-worker.js`: cache để hỗ trợ offline sau lần truy cập đầu
- `vercel.json`: cấu hình static site và HTTP headers
- `favicon.svg`: biểu tượng ứng dụng

## Chạy offline trên máy tính

Có thể mở bằng local web server. Ví dụ nếu máy có Python:

```bash
python -m http.server 8080
```

Sau đó mở `http://localhost:8080`.

> Không nên mở trực tiếp bằng `file://` nếu muốn kiểm thử Service Worker/PWA.

## Cập nhật dữ liệu

Mở `data.js` và sửa đúng bản ghi cần cập nhật. Mỗi khu phố gồm:

- `khuPho`
- `biThu.hoTen`
- `biThu.chucDanh`
- `biThu.dienThoai`
- `truongKhuPho.hoTen`
- `truongKhuPho.chucDanh`
- `truongKhuPho.dienThoai`
- `truSo`
- `viTriBanDo`: địa chỉ dùng để mở Google Maps và chỉ đường đến trụ sở

Không cần sửa `index.html` khi đổi dữ liệu.


## Chỉ đường đến trụ sở

Mỗi thẻ khu phố có nút **📍 Chỉ đường**. Nút này mở Google Maps với `viTriBanDo` làm điểm đến. Trên điện thoại, Google Maps có thể dùng vị trí hiện tại của người dùng làm điểm xuất phát nếu người dùng cho phép trong ứng dụng/bản đồ.

Nếu sau này có tọa độ GPS chính xác, có thể thay `viTriBanDo` bằng chuỗi `vĩ_độ,kinh_độ` để ghim điểm chính xác hơn. Không nên tự suy đoán tọa độ.

## Thêm khu phố

Thêm một object mới vào mảng `khuPhoData` trong `data.js`, tăng `id` liên tục. Nếu tổng số thay đổi, cập nhật các số thống kê trong `index.html`.

## Deploy Vercel

### Cách 1 - GitHub

1. Tạo repository GitHub mới.
2. Upload toàn bộ các file trong thư mục này lên repository.
3. Vào Vercel → Add New → Project.
4. Import repository vừa tạo.
5. Framework Preset: `Other`.
6. Không cần Build Command.
7. Không cần Output Directory.
8. Nhấn Deploy.

### Cách 2 - Vercel CLI

```bash
npm i -g vercel
vercel
```

Làm theo hướng dẫn trên màn hình.

## Deploy Netlify

Có thể kéo thả toàn bộ thư mục vào Netlify Drop. Đây là static site nên không cần build.

## Cập nhật phiên bản PWA

Khi thay đổi source và muốn buộc cache mới, đổi:

```js
const CACHE_NAME = 'my-ngai-directory-v1';
```

thành `v2`, `v3`... trong `service-worker.js`.

## SEO

Trong `index.html`, thay canonical placeholder:

```html
<link rel="canonical" href="https://your-domain.vercel.app/">
```

bằng domain Vercel thực tế sau khi deploy.
