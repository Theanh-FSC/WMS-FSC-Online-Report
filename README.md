# FSC Quản lý

Web xem báo cáo kho và vận hành của Food Supply Chain, có đăng nhập và phân quyền theo từng mục.
Dashboard FSC trên claude.ai vẫn là bản chính quản trị; web này **chỉ xem**.

## Các mục

| Mục | Nguồn dữ liệu |
|---|---|
| Tồn kho, Hàng nhập, Hàng xuất, Báo cáo kho (Nhập – Xuất – Tồn, cận date) | File `FSC_WMS_*.json` Dashboard ghi lên Drive → Sheet `FSC_WMS_Data` |
| Sản lượng KFM | Cùng file `FSC_WMS_*.json` (phần `kfm`) |
| Sản lượng CPG, Chấm công (bảng công + nhật ký quét) | Sheet `FSC_ChamCong_Data` |
| KPI FSC | Sheet "SCF - FSC: LỆCH HÀNG HÓA TẠI ST" |

Máy chủ là Apps Script `FSC_WMS.gs` gắn với Sheet `FSC_WMS_Data`, triển khai dạng Ứng dụng web.
Hằng `window.FSC_API_URL` trong `index.html` là URL `/exec` của Ứng dụng web đó.

## Phân quyền (tab `TaiKhoan` của Sheet `FSC_WMS_Data`)

- Cột C **Khách hàng**: mã khách hàng (`c-seedcom`), nhiều mã cách nhau dấu phẩy; `*` = nội bộ (thấy mọi khách hàng).
- Cột K → R, mỗi mục một cột: `Xem` hoặc `Sửa` (trống = không thấy mục đó).
  Tồn kho · Hàng nhập · Hàng xuất · Báo cáo kho · SL CPG · SL KFM · KPI FSC · Chấm công.
- Cột S **Xem tiền công**: `x` để thấy đơn giá và tiền công trong Sản lượng CPG.
- Các mục nội bộ (SL CPG, SL KFM, KPI FSC, Chấm công, tiền công) chỉ mở cho tài khoản có Khách hàng = `*`,
  kể cả khi lỡ ghi quyền cho tài khoản khách.
- Tài khoản chưa ghi quyền mục nào: chỉ xem 4 mục kho (như tài khoản khách hàng).
- Máy chủ kiểm tra quyền ở mọi lần tải dữ liệu, đổi quyền trong Sheet có hiệu lực ngay.
- `Sửa` hiện được lưu sẵn cho đợt sau (tạo phiếu nhập/xuất trên web); đợt này mọi mục đều chỉ xem.

Tạo tài khoản: thêm dòng, điền Tài khoản, Tên hiển thị, Khách hàng, Mật khẩu tạm và quyền.
Lần đầu đăng nhập bằng mật khẩu tạm, web bắt buộc đặt mật khẩu mới (ít nhất 6 ký tự).
Quên mật khẩu: xóa ô Mật khẩu (đã mã hóa), điền Mật khẩu tạm mới. Khóa: ghi `x` ở cột Khóa. Sai 5 lần: khóa 15 phút.
