# Bàn giao rà soát phần mềm — 10/10/2026

Đã chỉnh trực tiếp ứng dụng HTML hiện có, bổ sung tra cứu theo yêu cầu và bố cục ảnh mẫu. Không đổi công nghệ, không thay thế bộ đơn giá/tỷ lệ, không tính lại các hồ sơ lịch sử.

## 1. Phần mềm hiện tại và luồng dữ liệu

Ứng dụng gồm các luồng tính tiền phạt đất đai/môi trường, số lợi bất hợp pháp, tiền chuyển mục đích, tiền thuê đất; tra cứu thẩm quyền, bảng giá, biểu mẫu, hỏi đáp AI và quản lý hồ sơ. Dữ liệu người dùng nằm trong localStorage và IndexedDB của từng thiết bị/trình duyệt. Không có máy chủ lưu hồ sơ hoặc đồng bộ giữa thiết bị.

| Luồng | Đầu vào | Cách tính đang có trong mã | Đầu ra |
|---|---|---|---|
| Tiền phạt | Hành vi, khoản, khung diện tích/trường hợp, cá nhân/tổ chức, khu vực, mức tiền chọn trong khung | Nhân mức gốc theo đối tượng/khu vực; áp dụng trần cho khoản được cấu hình; mức ban đầu là trung bình hai đầu khung | Phạt, số lợi, tổng; lưu vụ việc, in phiếu, chuyển sang dự thảo |
| Số lợi đất đai | Diện tích từng lớp, G1/G2, ngày bắt đầu/kết thúc, số đã nộp | Tổng `(G2 − G1) × diện tích / 70 × số năm`, trừ đã nộp, tối thiểu 0; công thức lấn/chiếm hiện có dùng G1 = 0 | Bảng chi tiết từng lớp và phiếu tính |
| Số lợi môi trường | Lưu lượng, số ngày/giờ, đơn giá xử lý | Lưu lượng × thời gian × đơn giá | Số lợi, phiếu tính; nước dùng m³/ngày đêm, khí dùng m³/giờ |
| Chuyển mục đích | Diện tích/giá trước/giá sau từng lớp, trường hợp đất trước, thời hạn gốc/đã sử dụng nếu cần | Tổng tiền sau trừ tổng tiền trước, tối thiểu 0; tiền trước bằng 0 hoặc nhân phần thời hạn còn lại tùy lựa chọn | Tiền phải nộp, lưu hồ sơ, in/Word |
| Thuê hằng năm | Diện tích, giá đất, tỷ lệ %, miễn/giảm | Tổng diện tích × giá × tỷ lệ / 100; áp dụng giảm hoặc trình bày giai đoạn miễn theo lựa chọn | Mức tiền đồng/năm và ghi chú giai đoạn áp dụng |
| Thuê một lần | Diện tích, giá, số năm thuê, thời hạn của giá, miễn/giảm | Tổng diện tích × giá × số năm thuê / thời hạn của giá; miễn theo tỷ lệ thời gian hoặc giảm % | Tiền một lần, lưu hồ sơ, in/Word |

Các công thức trên mô tả **mã đang chạy**, chưa phải kết luận công thức đúng pháp luật. Dữ liệu pháp lý và đơn giá kèm trong HTML chưa được đối chiếu bản gốc có hiệu lực cho hồ sơ cụ thể.

## 2. Những thay đổi đã thực hiện

### Giao diện và tra cứu theo ảnh mẫu

- Giữ nền sáng, xanh đậm, phông chữ và thông tin bản quyền hiện có; đồng bộ biểu tượng nút thao tác bằng hệ SVG của ứng dụng. Giữ bộ favicon và biểu tượng điện thoại đã bổ sung.
- Thêm chỉ dẫn thứ tự làm việc ở ba màn hình tính tiền. Ô thông số chỉ đọc có nền riêng; kết quả tiền có nền xanh; thông báo dữ liệu chưa hợp lệ hiện sát khu vực kết quả.
- Tra cứu & Tổng hợp có ba thẻ: **Hồ sơ đã lưu**, **Bảng giá đất**, **Hành vi & mức xử phạt**.
- Hai mục mới dùng bố cục như ảnh: danh mục/bộ lọc cột trái, kết quả cột phải, danh mục có số lượng và nút thu gọn; bố cục chuyển thành một cột trên màn hình nhỏ.
- Tra cứu giá đất theo loại đất, xã/phường, vị trí/tuyến đường; phân trang 100 dòng để có thể xem hết kết quả. Không giới hạn toàn bộ kết quả ở 100 dòng.
- Tra cứu hành vi đất đai/môi trường theo nhóm và từ khóa; hiển thị các khoản/trường hợp, khung tiền theo cá nhân/tổ chức/khu vực, biện pháp khắc phục, xử phạt bổ sung và ghi chú hiện có. Bao gồm hành vi người dùng tự cập nhật.
- Khung phạt của tra cứu và màn hình tính phạt dùng chung một hàm, hạn chế hai nơi cho kết quả khác nhau.
- Bộ dữ liệu hiện tại không có mã nguồn văn bản/số trang cho từng dòng giá. Không tạo giả các cột này như trong ảnh mẫu.

### Tính toán và dữ liệu không hợp lệ

- Chặn số âm, số không hữu hạn, ô số trống/sai định dạng, diện tích/giá chính bằng 0 và kết quả vượt giới hạn biểu diễn số tiền của JavaScript.
- Chặn thời hạn gốc/mẫu số bằng 0, thời gian đã sử dụng lớn hơn thời hạn gốc, tỷ lệ giảm ngoài 0–100%, số năm miễn một lần vượt số năm thuê.
- Bỏ tỷ lệ lớp đất mặc định 60%/50%. Lớp mới cần nhập giá hoặc tỷ lệ có căn cứ; không tự suy ra giá. Bỏ thời hạn thuê/giá mặc định 50/70 năm ở màn hình thuê đất.
- Khi sửa giá thủ công, ngắt liên kết tỷ lệ của lớp đó. Khi xóa lớp gốc, giữ giá các lớp còn lại và bỏ tỷ lệ phụ thuộc lớp đã xóa.
- Tra tỷ lệ thuê đất theo tên xã/phường khớp chính xác; không lấy tỷ lệ của kết quả gần đúng đầu tiên. Tỷ lệ có sẵn trong bộ dữ liệu vẫn được giữ và cần người dùng đối chiếu quyết định áp dụng.
- Mức tiền phạt được kiểm tra trong khung đã chọn. Khi đổi hành vi, đối tượng, khu vực hoặc khung, kết quả trước bị vô hiệu hóa.
- Khi thay đổi đầu vào số lợi/chuyển mục đích/thuê đất, lưu/in/xuất không được dùng kết quả cũ. Số lợi bắt buộc theo dữ liệu hành vi cần được tính trước khi lưu kết quả tổng.
- Chặn trường hợp tổng diện tích lớp không khớp diện tích thực tế đã nhập, G2 nhỏ hơn G1, hoặc công thức lấn/chiếm đang cấu hình nhưng G1 khác 0; cần đối chiếu hồ sơ trước khi xử lý những trường hợp này.
- Ngày không tồn tại, thiếu ngày, ngày kết thúc không sau ngày bắt đầu bị chặn. Khoảng cuối tháng mà công thức cũ phân rã ra số ngày âm cũng bị chặn, chờ xác nhận quy tắc nghiệp vụ; không tự thay bằng cách chia số ngày/365.
- Đổi loại nước thải/khí thải xóa kết quả số lợi cũ. Sửa đơn vị lưu lượng nước trên màn hình và phiếu in thành m³/ngày đêm.

### Kết quả, báo cáo và bảo toàn dữ liệu

- Phiếu phạt và dự thảo tự điền phân biệt lĩnh vực đất đai/môi trường. Các viện dẫn chung trong biểu mẫu vẫn cần đối chiếu văn bản gốc trước khi ban hành.
- Tiền thuê hằng năm có đơn vị đồng/năm; đơn giá có đơn vị đồng/m²/năm. Bỏ việc lặp “đồng đồng/năm”.
- Miễn một số năm đầu: hiển thị 0 đồng/năm trong thời gian miễn và đánh dấu mức hằng năm sau miễn. Số tiền lưu vẫn là mức sau miễn theo công thức cũ, kèm nhãn `amountBasis`; không coi đó là tiền phải nộp ngay trong giai đoạn miễn.
- Tổng hợp tiền một lần, tiền hằng năm và tiền thuê chưa rõ kỳ được trình bày riêng. Biểu đồ tỷ lệ tiền chỉ so sánh các khoản một lần. CSV bổ sung kỳ/đơn vị và ghi chú, giữ giá trị số gốc.
- Sửa hồ sơ đã lưu vẫn được phép nhập tiền trực tiếp, nhưng chặn số âm/sai định dạng. Không tự viết lại số tiền lịch sử theo công thức mới.
- Sao lưu đầy đủ được kiểm tra cấu trúc trước khi khôi phục. Giá đất và biểu mẫu được ghi trong một giao dịch IndexedDB. Nếu ghi thất bại, hoàn tác thay đổi localStorage; nếu hoàn tác cũng thất bại, thông báo rõ cần bản sao lưu trước đó.
- Giá trị `null`/danh sách rỗng trong bản sao lưu có thể xóa đúng phần dữ liệu cũ tương ứng. Trường không có trong bản sao lưu cũ được giữ.
- Nhập danh sách hồ sơ không cộng lại bản sao trùng hoàn toàn; mã trùng với nội dung khác bị chặn trước khi ghi.
- Dữ liệu kèm HTML không ghi đè dữ liệu đã tồn tại khi khởi tạo. Giữ nguyên khóa lưu trữ, 22.828 dòng bảng giá, 31 biểu mẫu và vụ việc mẫu của nguồn đầu vào.
- Giữ biểu tượng nút sau thao tác sao lưu; bản HTML xuất kèm dữ liệu chứa mã đã sửa và xử lý chuỗi có ký tự `<` để không phá thẻ script.
- Nhập giá hàng loạt kiểm tra toàn bộ trước khi ghi, không thêm một phần rồi báo thành công. Giá dạng `1.500.000` không bị đổi thành 0; hướng dẫn nhập số không có dấu phân cách hàng nghìn, ví dụ `1500000`.

## 3. Kiểm tra đã thực hiện

**83 kiểm tra tự động đạt.** Công cụ kiểm tra dùng Node tích hợp và mô phỏng DOM/localStorage/IndexedDB. Bao gồm biên dịch JavaScript, bảo toàn dữ liệu nhúng, khuôn HTML xuất dữ liệu, phép tính, đầu vào không hợp lệ, lưu/in/xuất, khôi phục lỗi, 10 mục điều hướng và menu di động, tra cứu/lọc/phân trang và kết quả đọc bất đồng bộ.

Ngoài ra đã kiểm tra cân bằng thẻ và không trùng 29 ID tĩnh của khu vực Tra cứu & Tổng hợp. Ảnh mẫu đã được đọc để thực hiện bố cục. **Chưa chạy kiểm tra trực quan hoặc tương tác trên trình duyệt thật, chưa kiểm tra in giấy/Word thực tế, tải thư viện CDN, AI trực tuyến và môi trường Vercel sau triển khai bằng trình duyệt.** Việc kiểm tra triển khai qua trạng thái GitHub/Vercel không thay thế các kiểm tra đó.

### Ví dụ đối chiếu trước/sau

Các số dưới đây là dữ liệu thử có thể kiểm tra bằng phép tính; không phải đơn giá nghiệp vụ được xác nhận. Tất cả chín ví dụ giữ nguyên kết quả số trước và sau sửa.

| Ví dụ | Dữ liệu và phép tính | Kết quả trước = sau |
|---|---|---:|
| Số lợi đất đai | G2 = 1.000.000; G1 = 300.000; 100 m²; 01/01/2024–01/01/2026 = 2 năm; đã nộp 250.000. `(1.000.000−300.000)×100/70×2−250.000` | 1.750.000 đồng |
| Nước thải | 100 m³/ngày đêm × 30 ngày × 5.000 đồng/m³ | 15.000.000 đồng |
| Chuyển mục đích: giao đất | 100 m² × (2.000.000−300.000) | 170.000.000 đồng |
| Chuyển mục đích: thuê một lần | 100×2.000.000 − 100×300.000×(50−20)/50 | 182.000.000 đồng |
| Chuyển mục đích: tiền trước bằng 0 | 100×2.000.000 | 200.000.000 đồng |
| Thuê hằng năm | 100×2.000.000×0,5/100 | 1.000.000 đồng/năm |
| Thuê một lần | 100×2.000.000×50/70 | Giá trị gốc 142.857.142,85714287; hiển thị 142.857.143 đồng |
| Thuê hằng năm giảm 20% | 1.000.000×(1−20/100) | 800.000 đồng/năm |
| Thuê một lần miễn 3/50 năm | (100×2.000.000×50/70)×(1−3/50) | Giá trị gốc 134.285.714,2857143; hiển thị 134.285.714 đồng |

Kết quả kiểm tra từng ca và các số trước/sau nằm trong `tests/verification-results.json`; mã chạy trong `tests/review.test.cjs`.

### Làm tròn

Giữ cách hiện có: phép tính và hồ sơ lưu giữ số gốc; tiền trên màn hình/phiếu dùng `Math.round` đến đồng; giá suy ra từ tỷ lệ lớp được làm tròn đến đồng/m² theo hàm cũ. Không âm thầm đổi cách làm tròn số tiền lịch sử. Cộng các thành phần đã làm tròn trên màn hình có thể lệch một đồng so với làm tròn tổng số gốc. Cần quy tắc nghiệp vụ chính thức để quyết định làm tròn từng dòng, từng khoản hay tổng cuối cùng.

## 4. Cần bổ sung để kết luận nghiệp vụ

1. Quyết định bảng giá đất, phụ lục giá, quyết định tỷ lệ thuê đất và ngày hiệu lực/địa bàn áp dụng; chỉ rõ bản nào tương ứng 22.828 dòng hiện có. Bổ sung mã văn bản/số trang từng dòng nếu muốn tra nguồn như ảnh.
2. Văn bản gốc đang áp dụng cho xử phạt đất đai/môi trường, số lợi bất hợp pháp, thẩm quyền và chuyển mục đích/thuê đất; bao gồm các văn bản sửa đổi đang được HTML viện dẫn. Viện dẫn có sẵn không đủ để xác nhận còn hiệu lực hoặc phù hợp vụ việc.
3. Hồ sơ/mẫu tính đã được kiểm tra, gồm diện tích, loại đất, ngày vi phạm/quyết định, chủ thể, giá áp dụng, thời hạn, miễn/giảm, số đã nộp và kết quả mong đợi.
4. Quy tắc quy đổi ngày/tháng/năm ở cuối tháng, cùng ngày, phần lẻ dưới/trên 15 ngày; quy tắc làm tròn tiền và giá theo từng bước.
5. Cách xử lý hợp lệ khi G1/G2 hoặc đơn giá bằng 0, G2 < G1, nhiều thời kỳ giá, nhiều hành vi, lựa chọn khung phạt theo lượng/diện tích và các ngoại lệ chủ thể. Bản này không tự chọn khung chỉ từ con số người dùng nhập.
6. Mẫu biểu chính thức và phạm vi biện pháp khắc phục theo từng khoản/điểm. Dữ liệu hiện có chủ yếu gắn biện pháp ở cấp hành vi, chưa đủ kết luận mọi biện pháp áp dụng cho tất cả khoản trong hành vi đó.

Chưa thể kết luận phần mềm đúng toàn bộ quy định pháp luật chỉ từ mã và dữ liệu kèm sẵn. Bản này khắc phục lỗi kỹ thuật có thể kiểm chứng và ghi rõ các điểm cần căn cứ nghiệp vụ.

## 5. Cập nhật và sử dụng

1. Trên địa chỉ phần mềm đang dùng, **Xuất sao lưu** trước khi cập nhật. Giữ file JSON ở nơi an toàn.
2. Bản mã nguồn gồm `index.html`, bộ biểu tượng và manifest, README, báo cáo này và thư mục kiểm tra. Không cần cài gói hoặc chạy build để dùng ứng dụng.
3. GitHub `letunglam1992/tinhtiendatdai`, nhánh `main`, được cập nhật; dự án Vercel đã liên kết GitHub có thể tự triển khai commit mới. Nếu cần thủ công, chọn commit mới ở Vercel và Redeploy; Framework **Other**, không Build Command, phục vụ thư mục gốc.
4. Mở lại **cùng địa chỉ và trình duyệt**, tải lại trang. Dữ liệu đã lưu được giữ; bản cập nhật không chạy di chuyển hoặc tính lại hồ sơ. Đổi địa chỉ/thiết bị: xuất ở địa chỉ cũ và nhập ở địa chỉ mới.
5. Vào **Tra cứu & Tổng hợp**, chọn thẻ cần dùng. Giá đất: chọn danh mục hoặc nhập bộ lọc, bấm Tra cứu; dùng Trang trước/sau. Hành vi: chọn lĩnh vực, đối tượng, khu vực, nhóm hoặc từ khóa; mở nhóm kết quả để xem từng khung phạt và biện pháp.
6. Trước sử dụng chính thức, kiểm tra bằng trình duyệt của đơn vị: tính/lưu một hồ sơ mẫu; sửa đầu vào và xác nhận yêu cầu tính lại; in/Word; xuất/nhập sao lưu trên hồ sơ thử; tra cứu giá và hành vi; xem trên điện thoại. Đối chiếu kết quả với hồ sơ nghiệp vụ gốc.

Chạy lại kiểm tra kỹ thuật từ thư mục mã nguồn: `node tests/review.test.cjs`. Để đối chiếu với bản trước sửa, truyền thêm đường dẫn HTML của bản đó: `node tests/review.test.cjs /duong-dan/ban-truoc.html`.

Nếu phải quay lại bản trước, phục hồi commit `386cf3f7b380e5d71b8d9e0eec58551a3ac3f78e` hoặc deployment tương ứng trên Vercel. Không xóa bộ nhớ trình duyệt khi quay lại. Khôi phục dữ liệu từ JSON đã sao lưu nếu đã nhập/ghi đè dữ liệu trong quá trình sử dụng.

Tác giả và bản quyền của phần mềm được giữ theo nguồn: Tùng Lâm — Sở Nông nghiệp và Môi trường tỉnh Sơn La; 0987559092.
