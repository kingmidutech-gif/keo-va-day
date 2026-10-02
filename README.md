# Trang chương trình Kéo và Đẩy

Trang giới thiệu chương trình Kéo và Đẩy của Công ty Cổ phần Midu Group, kèm trang nhận hồ sơ đề xuất em nhỏ cần hỗ trợ.

Toàn bộ là trang tĩnh, không có khung nền tảng nào, không cần cài đặt gì. Mở `index.html` bằng trình duyệt là xem được ngay.

## Tệp trong kho

```
index.html          trang giới thiệu chương trình
dang-ky.html        trang điền hồ sơ đề xuất
logo-keo-day.png    logo chương trình, nền trong suốt
anh/banner.jpg      ảnh đầu trang, 2400 x 1000
anh/chu-tich.jpg    ảnh dọc, dùng ở khối lời phát biểu
anh/la-01..13.jpg   13 ảnh hoạt động, xếp thành lưới cuối trang
```

Mọi CSS và JavaScript nằm ngay trong hai tệp HTML, không tách ra tệp riêng. Sửa giao diện thì sửa trong thẻ `<style>` ở đầu tệp, sửa cách chạy thì sửa trong thẻ `<script>` ở cuối tệp.

## Hồ sơ đi đường nào

```
dang-ky.html
   người điền chọn ảnh từ máy
   trình duyệt thu ảnh về cạnh dài 1500px, nén JPEG chất lượng 0.8
   gửi POST một gói JSON tới Apps Script
        |
        v
Apps Script  (gắn trong Google Sheet nhận hồ sơ)
   tạo thư mục con trong Drive, cất 6 ảnh
   ghi 29 cột vào tab "Câu trả lời biểu mẫu 2"
   bắn tin vào nhóm Zalo ekip qua Smax
   trả về { ok: true, ma: <số thứ tự hồ sơ> }
```

Mã nguồn Apps Script không nằm trong kho này, nó nằm trong chính Google Sheet nhận hồ sơ, mở bằng Tiện ích mở rộng rồi chọn Apps Script. Bản sao để đối chiếu nằm ở `viec/keo-va-day/apps-script.gs` trong kho tài liệu nội bộ.

## Những chỗ hay phải sửa

**Đổi địa chỉ Apps Script.** Mở `dang-ky.html`, tìm dòng gần cuối tệp:

```javascript
var API = "https://script.google.com/macros/s/..../exec";
```

Địa chỉ này đổi mỗi khi tạo bản triển khai mới trong Apps Script. Sửa code bên Apps Script mà quên bấm Triển khai thì web vẫn chạy code cũ.

**Thêm hoặc bớt trường trong hồ sơ.** Phải sửa cả ba chỗ cho khớp nhau:

1. `dang-ky.html` phần HTML, thêm ô nhập với `id` theo quy ước: `a*` người đề xuất, `b*` em nhỏ, `c*` bố hoặc mẹ, `d*` ảnh.
2. `dang-ky.html` phần JavaScript, thêm `id` đó vào mảng thu thập dữ liệu và viết câu kiểm tra tương ứng.
3. Apps Script, thêm vào mảng 29 cột trong hàm `nhanHoSo`, nhớ sửa luôn con số `29` trong `getRange(dong, 1, 1, 29)`.

**Đổi ảnh lưới cuối trang.** Thay tệp trong `anh/` giữ nguyên tên, hoặc sửa danh sách thẻ `img` trong mục `.la` của `index.html`. Ô đầu tiên có thêm lớp `lon` nên chiếm hai hàng hai cột.

**Ảnh nặng.** Nén trước khi đưa vào kho. Ảnh lưới nên để quanh 760 x 570 và dưới 100 KB mỗi tệp.

## Lưu ý khi làm trang quản trị hồ sơ

Trang quản trị sẽ hiển thị số căn cước và ảnh căn cước hai mặt của trẻ em lẫn bố mẹ, ảnh sổ hộ nghèo, địa chỉ nhà. Đây là dữ liệu cá nhân nhạy cảm của trẻ em.

**Bắt buộc có đăng nhập.** Không được để ai có đường dẫn cũng mở được. Cách nhẹ nhất là bật Cloudflare Access cho đường dẫn `/quan-tri`, miễn phí tới 50 người, chặn bằng danh sách email, không phải tự viết phần đăng nhập.

**Không nhúng khoá vào trang tĩnh.** Mã Smax và mọi khoá khác phải nằm bên Apps Script, không được đưa vào HTML.

**Mỗi hồ sơ một đường dẫn riêng.** Apps Script trả về `ma` là số thứ tự hồ sơ, đúng bằng số dòng trong Sheet trừ đi một. Dự kiến mỗi em một trang theo dạng `/ho-so/<ma>`. Khi làm xong, điền đường dẫn gốc vào biến `LINK_QUAN_TRI` trong Apps Script, tin nhắn Zalo sẽ tự kèm đường dẫn tới đúng hồ sơ vừa gửi.

**Ảnh nằm trong Google Drive**, trong thư mục "Hồ sơ Kéo và Đẩy", mỗi hồ sơ một thư mục con tên `HS<mã> - <tên bé>`. Đường dẫn từng ảnh nằm ở sáu cột cuối của Sheet.

## Đưa lên web

Kho này nối với Cloudflare Pages. Lưu thay đổi lên nhánh chính là trang tự cập nhật sau khoảng một phút.

Thư mục dựng để trống, thư mục xuất bản là thư mục gốc của kho.
