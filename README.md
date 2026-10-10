# Tính tiền đất đai và môi trường

Ứng dụng HTML hỗ trợ tính tiền xử phạt vi phạm hành chính trong lĩnh vực đất đai và môi trường, tiền chuyển mục đích sử dụng đất, tiền thuê đất; tra cứu thẩm quyền, bảng giá đất, biểu mẫu và quản lý hồ sơ.

**Tác giả: Tùng Lâm – Sở Nông nghiệp và Môi trường tỉnh Sơn La.**  
Liên hệ: 0987559092. © 2026 Tùng Lâm. All rights reserved.

## Sử dụng

Mở `index.html` bằng trình duyệt hoặc triển khai thư mục này trên dịch vụ phục vụ website tĩnh. Không cần lệnh build. Với Vercel: chọn Framework Preset **Other**, bỏ trống Build Command, dùng thư mục gốc của kho.

Giao diện đồng bộ với phần mềm Điều hành nhiệm vụ: menu bên trái, nền sáng, xanh đậm, thẻ thống kê, thanh công cụ sao lưu và hộp Giới thiệu, bản quyền. Menu điện thoại hỗ trợ bàn phím và tự đóng khi chuyển chức năng; nút logo đưa về Tổng quan.

## Dữ liệu

- File giữ nguyên dữ liệu kèm bản người dùng cung cấp: 22.828 dòng bảng giá đất, 31 biểu mẫu và 1 vụ việc mẫu.
- Dữ liệu người dùng lưu riêng trên từng thiết bị/trình duyệt bằng localStorage và IndexedDB. Không tự đồng bộ qua máy chủ hay Google Sheets.
- **Xuất sao lưu / Nhập sao lưu** dùng để chuyển dữ liệu. **Xuất bản kèm dữ liệu** tạo một file HTML chứa dữ liệu hiện tại và giữ giao diện mới.
- Khóa Gemini nhập trong ứng dụng không nằm trong gói sao lưu hoặc bản xuất kèm dữ liệu.
- Đổi địa chỉ website không tự chuyển dữ liệu đã lưu ở địa chỉ cũ. Hãy xuất sao lưu ở địa chỉ cũ rồi nhập ở địa chỉ mới.
- Đọc Excel và Word sử dụng thư viện XLSX/Mammoth từ CDN như bản gốc, cần kết nối mạng để tải hai thư viện này.

## Phạm vi bản cập nhật 10/10/2026

Chỉnh giao diện, bố cục, điều hướng, thống kê tổng quan và thông tin bản quyền. Giữ các hàm tính toán nghiệp vụ và bộ dữ liệu pháp lý của file nguồn. Bản cập nhật không thực hiện thẩm định tính đúng đắn hoặc tính còn hiệu lực của các căn cứ pháp luật.

Kiểm tra: cú pháp JavaScript, cấu trúc HTML, bảo toàn dữ liệu và khóa lưu trữ, phép tính mẫu và mã điều hướng bằng môi trường mô phỏng DOM. Chưa kiểm tra trực quan bằng trình duyệt thật trên máy tính/điện thoại trong môi trường thực hiện này.

## Bản quyền

Phần mềm được bảo hộ quyền tác giả. Nghiêm cấm sao chép, sửa đổi, phân phối, dịch ngược hoặc khai thác thương mại khi chưa được tác giả cho phép, trừ trường hợp pháp luật có quy định khác. Thư viện bên thứ ba tuân theo giấy phép riêng của từng thư viện.

Công cụ hỗ trợ tra cứu, tham khảo nội bộ; không thay thế văn bản pháp luật gốc và ý kiến chuyên môn của người có thẩm quyền.
