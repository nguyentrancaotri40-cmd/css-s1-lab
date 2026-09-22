# Lab S1 — Mô hình đe dọa trang bán hàng nhỏ

**Học phần An toàn hệ thống máy tính · Buổi 1 · Nguyễn Trần Cao Trí**

> Đề bài gốc Lab S1 của giảng viên được giữ nguyên ở tệp `README-de-bai.md` trong cùng thư mục. Đây là bài nộp cá nhân.

## Hệ thống đã chọn

Trang bán hàng nhỏ (mini e-commerce) phục vụ khách mua lẻ, gồm ba thành phần:

1. **Trình duyệt người mua** — giao diện khách hàng duyệt sản phẩm, thêm vào giỏ, đặt hàng, thanh toán qua HTTPS.
2. **Máy chủ ứng dụng Flask** — xử lý nghiệp vụ, xác thực người dùng, gọi cơ sở dữ liệu, trả về HTML hoặc JSON.
3. **Cơ sở dữ liệu PostgreSQL** — lưu trữ tài khoản, sản phẩm, đơn hàng và thông tin thanh toán.

Đây không phải hệ thống thật của bất cứ tổ chức nào; đây là mô hình thu nhỏ dùng cho bài tập.

## Ba mối đe dọa được chọn để xử lý

| Mã | Mối đe dọa | Nguyên lý | ATT&CK | Tác động | Khả năng | Tích |
|---|---|---|---|---|---|---|
| M01 | SQL injection qua ô tìm kiếm sản phẩm | 2 | T1190 | 5 | 4 | **20** |
| M02 | IDOR — đọc đơn hàng của người mua khác | 2 | T1078 | 5 | 4 | **20** |
| M03 | Sửa cookie phiên thành admin | 5 | T1548 | 5 | 4 | **20** |

## Vì sao chọn ba mối này

Ba mối M01, M02, M03 đều có **tích tác động × khả năng = 20**, cao nhất trong tám mối đã liệt kê. Chúng tôi xếp hạng theo tích này thay vì chỉ theo tác động, vì một mối có tác động 5 nhưng khả năng 1 hiếm khi xảy ra thì ưu tiên xử lý thấp hơn một mối có tác động 5 và khả năng 4.

Cả ba mối đều là **lỗi kiểm soát truy cập** — nhóm lỗi phổ biến nhất trong các ứng dụng web thương mại điện tử, và hậu quả trực tiếp là rò rỉ dữ liệu khách hàng hoặc chiếm tài khoản. Chi phí xử lý ước lượng lần lượt là 8, 6 và 4 giờ người, tổng 18 giờ — nằm trong ngân sách một tuần công của một lập trình viên.

## Phần bị bỏ lại

Năm mối còn lại (M04 brute force, M05 XSS lưu trữ, M06 lộ thông tin thanh toán trong log, M07 CSRF, M08 DoS) đều có tích từ 12 tới 15, thấp hơn nhóm được chọn. Đáng chú ý là **M06 có tác động 5** — ngang với ba mối được chọn — nhưng khả năng chỉ 3, nên tích là 15 và bị xếp sau. Nếu nguồn lực cho phép, M06 nên được xử lý ngay sau ba mối này vì hậu quả rò rỉ dữ liệu thẻ là nghiêm trọng về mặt pháp lý.

## Ghi chú về chọn hệ thống

Chúng tôi chọn hệ thống ba thành phần thay vì một hệ thống lớn hơn vì mục tiêu của bài là **chất lượng phát biểu kiểm được**, không phải độ phủ của mô hình. Một mô hình sắc cho hệ thống ba thành phần có giá trị hơn một mô hình hời hợt cho hệ thống mười thành phần.

## Sản phẩm kèm theo

- `docs/threat-model.json` — mô hình đe dọa đầy đủ (8 mối, 3 mối được chọn).
- `evidence/S1/preflight.txt` — bằng chứng `make preflight` đã chạy.
- `README-de-bai.md` — đề bài gốc của giảng viên, giữ nguyên.