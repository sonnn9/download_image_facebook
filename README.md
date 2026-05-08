# download_image_facebook

Tool dùng [gallery-dl](https://github.com/mikf/gallery-dl) để download toàn bộ ảnh từ các link Facebook trong [links.txt](links.txt).

## 1. Cài đặt (đã làm 1 lần)

```powershell
C:\Python313\python.exe -m pip install -U gallery-dl
```

## 2. Export cookies Facebook (BẮT BUỘC)

Facebook yêu cầu phải đăng nhập, nên cần cookies. Cách dễ nhất:

1. Cài extension **"Get cookies.txt LOCALLY"** cho Chrome/Edge (https://chromewebstore.google.com/detail/get-cookiestxt-locally/cclelndahbckbenkjhflpdbgdldlbecc).
2. Đăng nhập vào https://www.facebook.com bằng tài khoản của bạn.
3. Click icon extension → Export → format **Netscape** → lưu file vào thư mục project với tên đúng là `cookies.txt`.

> ⚠️ `cookies.txt` đã được thêm vào `.gitignore`, không commit lên git.

## 3. Bỏ link vào `links.txt`

Mỗi link 1 dòng. Hỗ trợ:
- Photo đơn: `https://www.facebook.com/photo.php?fbid=...`
- Album: `https://www.facebook.com/media/set/?set=...`
- Profile / Page (lấy toàn bộ ảnh public): `https://www.facebook.com/<username>/photos`
- Post: `https://www.facebook.com/<username>/posts/<id>`

Dòng bắt đầu bằng `#` được coi là comment.

## 4. Chạy

```powershell
.\download.ps1
```

Ảnh được lưu vào [downloads/facebook/](downloads/) theo cấu trúc `downloads/facebook/<username-hoặc-set-id>/`.

File `.archive.sqlite3` ghi nhớ ảnh đã tải để lần sau chạy lại không tải trùng.

## Tuỳ chỉnh

Sửa [gallery-dl.conf](gallery-dl.conf):
- `"videos": true` để tải kèm video.
- `"directory"` / `"filename"` để đổi cấu trúc thư mục, tên file.
- `"sleep"` để tăng delay nếu bị Facebook rate-limit.

## Troubleshooting

- **`HttpError: 401 / 403`** → cookies hết hạn, export lại `cookies.txt`.
- **`No suitable extractor`** → link không phải định dạng FB hỗ trợ (vd: link share rút gọn). Mở link trong browser, copy URL thật rồi dán lại.
- **Tải 1 link đơn lẻ để test:**
  ```powershell
  C:\Python313\python.exe -m gallery_dl --config .\gallery-dl.conf "<URL>"
  ```
