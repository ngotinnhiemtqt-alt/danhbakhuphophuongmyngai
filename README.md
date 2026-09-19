[README.md](https://github.com/user-attachments/files/32409257/README.md)
# Danh bạ Phường Mỹ Ngãi - V3

Ứng dụng web static/PWA chạy trực tiếp trên Vercel hoặc Netlify. Phiên bản V3 giữ phong cách giao diện hiện tại và bổ sung danh bạ cán bộ, công chức, bản đồ, QR, định vị, tính khoảng cách và trang quản trị dữ liệu cục bộ.

## Dữ liệu đã tích hợp

- 12 Khu phố hiện hành với dữ liệu Bí thư Chi bộ và Trưởng Khu phố của phiên bản cập nhật riêng.
- 132 bản ghi cán bộ, công chức và đơn vị thuộc các mục A-G của file danh bạ được cung cấp.
- Không nhập đè các mục H (các khóm/khu phố cũ) và I (Trưởng Khu phố) trong PDF vì các mục này đã có dữ liệu cập nhật riêng trong ứng dụng.
- 108/132 bản ghi cán bộ có số điện thoại trong tài liệu nguồn; các bản ghi không có số được hiển thị "Chưa có số điện thoại trong danh bạ nguồn".

## Chức năng mới

### Bản đồ và chỉ đường

Mỗi Khu phố có:

- `Xem bản đồ & QR`: mở bản đồ Google ngay trong ứng dụng.
- `Chỉ đường`: mở Google Maps với trụ sở làm điểm đến.
- QR mở vị trí trên điện thoại.
- Hỗ trợ tọa độ GPS chính xác nếu đã được thiết lập.

Nếu chưa có GPS xác nhận, ứng dụng ghim theo `viTriBanDo`/địa chỉ và không tự bịa tọa độ.

### Tính khoảng cách

Người dùng bấm **Dùng vị trí của tôi** để cho phép trình duyệt lấy vị trí hiện tại. Vị trí chỉ được giữ trong bộ nhớ của phiên đang mở, không được gửi về server của ứng dụng.

Khoảng cách đường chim bay chỉ được tính khi trụ sở có tọa độ GPS chính xác. Chỉ đường thực tế được mở bằng Google Maps.

### Danh bạ cán bộ, công chức

Tab **Cán bộ, công chức** cho phép:

- tìm không dấu theo họ tên, chức danh, đơn vị, bộ phận hoặc số điện thoại;
- lọc theo cơ quan/đơn vị và nhóm;
- gọi điện và sao chép số điện thoại.

### Trang quản trị `/admin.html`

Trang quản trị hỗ trợ:

- tự thiết lập mã PIN cục bộ ở lần truy cập đầu;
- sửa dữ liệu 12 Khu phố;
- nhập tọa độ GPS thủ công;
- đứng tại trụ sở và bấm **Lấy GPS tại vị trí hiện tại**;
- sửa/thêm/xóa danh bạ cán bộ, công chức;
- sửa thông tin UBND Phường;
- xuất/nhập backup JSON;
- xuất `data.js` mới để đưa lên GitHub/Vercel.

**Quan trọng:** vì đây là website static, thay đổi trong `/admin.html` được lưu bằng `localStorage` và chỉ có hiệu lực trên trình duyệt đó. Để công khai thay đổi cho mọi người dùng, hãy bấm **Xuất data.js**, thay file `data.js` trong repository GitHub và commit để Vercel deploy lại. Muốn quản trị online đa thiết bị và lưu tức thời cần bổ sung backend/database có xác thực.

## Cấu trúc source

- `index.html`: giao diện tra cứu công khai
- `style.css`: giao diện chính, responsive, light/dark
- `data.js`: dữ liệu nguồn 12 Khu phố + 132 cán bộ, công chức
- `app.js`: tìm kiếm, lọc, gọi/copy/share, bản đồ, QR, khoảng cách, PWA
- `admin.html`: giao diện quản trị
- `admin.css`: CSS quản trị
- `admin.js`: sửa dữ liệu, GPS, backup/import/export
- `manifest.json`: cấu hình PWA
- `service-worker.js`: cache ứng dụng
- `vercel.json`: headers và quyền geolocation khi người dùng chủ động yêu cầu
- `favicon.svg`: biểu tượng ứng dụng

## Thiết lập GPS chính xác cho 12 trụ sở

1. Deploy website lên HTTPS (Vercel).
2. Mở `/admin.html` trên điện thoại.
3. Thiết lập/đăng nhập PIN.
4. Chọn **Khu phố & GPS**.
5. Chọn khu phố cần cập nhật.
6. Đứng tại đúng trụ sở.
7. Bấm **Lấy GPS tại vị trí hiện tại** và cho phép quyền vị trí.
8. Kiểm tra nút **Kiểm tra trên Google Maps**.
9. Bấm **Lưu thay đổi**.
10. Sau khi cập nhật đủ 12 điểm, bấm **Xuất data.js** và thay file trên GitHub để công khai tọa độ cho mọi người dùng.

Không nên lấy tọa độ ước lượng từ tên đường nếu mục tiêu là ghim chính xác vị trí trụ sở.

## Chạy thử trên máy tính

```bash
python -m http.server 8080
```

Mở `http://localhost:8080`. Geolocation có thể hoạt động trên localhost, nhưng khi deploy thực tế phải dùng HTTPS.

## Deploy Vercel

1. Upload toàn bộ file lên một repository GitHub.
2. Vào Vercel -> Add New -> Project.
3. Import repository.
4. Framework Preset: `Other`.
5. Không cần Build Command.
6. Không cần Output Directory.
7. Deploy.

Sau khi có domain thật, thay canonical trong `index.html`:

```html
<link rel="canonical" href="https://your-domain.vercel.app/">
```

bằng URL thực tế.

## PWA / cache

Mỗi khi cập nhật source lớn, tăng tên cache trong `service-worker.js`, ví dụ `my-ngai-directory-v4`, để thiết bị nhận phiên bản mới nhanh hơn.
