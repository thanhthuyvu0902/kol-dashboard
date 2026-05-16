# KOL TikTok Dashboard - LOHO House

Dashboard công khai: **`https://lohohouse.github.io/kol-dashboard/`** (sau khi deploy)

Nguồn dữ liệu: Google Sheets `KOL_TikTok_Database_PerMonth`

## Cách tự cập nhật

1. Vào Google Sheets `KOL_TikTok_Database_PerMonth`
2. Tạo 3 sheet cho tháng mới — đặt tên đúng format: `T5_DonHang`, `T5_Video`, `T5_SanPham` (đổi 5 thành số tháng)
3. Đảm bảo cấu trúc cột giống các tháng trước
4. Đợi tối đa 1 giờ — GitHub Actions tự fetch + cập nhật dashboard

Nếu muốn update ngay: vào tab **Actions** → chọn **Update KOL Dashboard** → bấm **Run workflow**.

## Cấu trúc file

| File | Mô tả |
|---|---|
| `index.html` | Dashboard - GitHub Pages serve file này |
| `update_kol_dashboard.py` | Script tải Sheets → generate `index.html` |
| `kol_dashboard_template.html` | Khung HTML (giao diện) |
| `.github/workflows/update.yml` | Lịch chạy auto-update (mỗi giờ) |

## Setup ban đầu (chỉ làm 1 lần)

### Bước 1: Tạo repo GitHub
- Vào https://github.com/lohohouse (hoặc account chị đang dùng)
- New repository → tên `kol-dashboard` → Public → Create
- Trên máy: tải toàn bộ folder `kol-dashboard-deploy` này
- Upload lên repo (kéo thả vào GitHub web, hoặc dùng GitHub Desktop)

### Bước 2: Bật GitHub Pages
- Vào repo → **Settings** → **Pages**
- Source: **Deploy from a branch**
- Branch: **main** / folder **/ (root)** → Save
- Đợi 1-2 phút → URL hiện ra: `https://lohohouse.github.io/kol-dashboard/`

### Bước 3: Kiểm tra Sheets phải ở chế độ public-view
- Vào Google Sheets → **Share** → đổi sang **"Anyone with the link – Viewer"**
- Nếu không thì GitHub Actions không tải được file

### Xong! Link công khai có thể chia sẻ cho team / khách hàng

## Đổi lịch cập nhật

Mở `.github/workflows/update.yml`, sửa dòng:
```yaml
- cron: '0 * * * *'   # mỗi giờ (mặc định)
```
Một số lựa chọn:
- `'0 */2 * * *'` — mỗi 2 giờ
- `'0 1,9 * * *'` — 8h sáng và 16h chiều VN
- `'0 1 * * *'` — 8h sáng VN mỗi ngày (UTC+7)

## Bộ lọc trong dashboard

- **Trạng thái đơn**: click pill để bật/tắt (mặc định "Đã quyết toán")
- **Khoảng ngày**: chọn date range
- **Loại nội dung**: dropdown ở bảng tăng trưởng (Video / Trưng bày / LIVE / Chương trình lưu lượng)

## 7 tab dashboard

| Tab | Nội dung |
|---|---|
| 總覽 Tổng Quan | KPI · bảng tăng trưởng có filter loại ND · chart so sánh GMV/Đơn · Top 15 KOL · trạng thái · loại ND · Top SP |
| Tháng 1-N | Chi tiết từng tháng có ▲▼ so tháng trước |
| 🔍 Tra Cứu | Tùy chỉnh khoảng ngày tự do |
| 達人 KOL | Toàn bộ KOL sort theo GMV |
