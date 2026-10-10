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

Đã rà soát và sửa kiểm tra đầu vào, kết quả cũ khi lưu/in/xuất, đơn vị tiền thuê, phân biệt phiếu đất đai/môi trường, tổng hợp tiền theo kỳ và khôi phục sao lưu. Giữ số gốc cho dữ liệu hợp lệ, không thay bộ đơn giá/tỷ lệ và không tính lại hồ sơ cũ. Lớp đất mới không có tỷ lệ mặc định 60%/50%; thời hạn thuê/giá cần nhập theo hồ sơ.

**Tra cứu & Tổng hợp** có ba mục: Hồ sơ đã lưu, Bảng giá đất, Hành vi & mức xử phạt. Bố cục theo ảnh mẫu: thẻ chọn ở trên, danh mục và bộ lọc bên trái, bảng kết quả bên phải. Tra cứu giá có phân trang; tra cứu hành vi hiển thị các khung phạt, biện pháp khắc phục và xử phạt bổ sung theo dữ liệu hiện có.

**83 kiểm tra tự động đạt**, gồm 9 ví dụ so sánh trước/sau, lưu/in, sao lưu/khôi phục và tra cứu. Chạy `node tests/review.test.cjs`. Kết quả chi tiết ở `tests/verification-results.json`. Chưa kiểm tra trực quan trên trình duyệt thật, in/Word thực tế, thư viện CDN hoặc AI trực tuyến.

Đọc [báo cáo rà soát và hướng dẫn cập nhật](BAO_CAO_RA_SOAT.md) để xem công thức, các thay đổi, ví dụ và thông tin còn thiếu. Chưa xác nhận tính đúng pháp luật: cần văn bản gốc, quyết định giá/tỷ lệ và quy tắc quy đổi thời gian/làm tròn áp dụng. Khoảng ngày cuối tháng mà công thức cũ cho số ngày âm bị chặn, chờ căn cứ nghiệp vụ.

Trước cập nhật, xuất sao lưu JSON. Giữ cùng địa chỉ website/trình duyệt để tiếp tục dùng dữ liệu. Kho GitHub `main` liên kết Vercel; cập nhật mã kích hoạt triển khai. Không có lệnh build.

## Bản quyền

Phần mềm được bảo hộ quyền tác giả. Nghiêm cấm sao chép, sửa đổi, phân phối, dịch ngược hoặc khai thác thương mại khi chưa được tác giả cho phép, trừ trường hợp pháp luật có quy định khác. Thư viện bên thứ ba tuân theo giấy phép riêng của từng thư viện.

Công cụ hỗ trợ tra cứu, tham khảo nội bộ; không thay thế văn bản pháp luật gốc và ý kiến chuyên môn của người có thẩm quyền.

## Biểu tượng website

Biểu tượng cán cân trắng trên nền xanh đậm dùng cho tab trình duyệt, dấu trang và màn hình chính điện thoại. Có SVG, ICO, PNG 16/32/180/192/512 và manifest; không bổ sung chế độ chạy ngoại tuyến. Nếu biểu tượng cũ còn lưu trong bộ nhớ đệm, tải lại trang hoặc xóa lối tắt rồi thêm lại.
