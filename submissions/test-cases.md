# Test Cases — Bảng trường hợp kiểm thử

> **Hướng dẫn**: Viết tối thiểu **20 TC** phủ đủ các chức năng chính (REQ-01 → REQ-08).
> Xem [examples/sample-test-case.md](../examples/sample-test-case.md) để hiểu cách viết TC tốt.
> Tự tổ chức và phân nhóm test case theo cách hợp lý nhất.

| Thông tin | |
|---|---|
| **Nhóm** | Group 26 |
| **Ngày tạo** | 20/05/2026 |
| **Hệ thống** | https://stqa.rbc.vn |
| **Tham chiếu** | SRS v1.0 |

---

## Bước 1: Mô hình hóa miền đầu vào — Input Domain Modeling (IDM)

> 📖 **Textbook:** Chương 6 — *Input Domain Modeling*, Paul Ammann & Jeff Offutt.
>
> **Trước khi viết Test Case**, nhóm **phải** phân tích miền đầu vào bằng bảng IDM bên dưới.
> Mỗi chức năng cần xác định: **Đặc tính (Characteristic)**, **Phân vùng (Block/Partition)**, và **Giá trị đại diện (Value)**.

### IDM — Đăng nhập (REQ-01)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Email có tồn tại trong DB? | Có | `librarian@library.com` | Đăng nhập thành công |
| | Không | `noone@email.com` | Thông báo lỗi |
| Mật khẩu có đúng? | Đúng | `admin123` | Đăng nhập thành công |
| | Sai | `wrongpass` | Thông báo lỗi |
| Ô nhập có rỗng? | Không rỗng | (giá trị bất kỳ) | Xử lý bình thường |
| | Rỗng | `""` | Thông báo "Vui lòng nhập..." |

### IDM — Tìm kiếm sách (REQ-03)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Từ khóa có tồn tại trong DB? | Có (tên sách) | `"Flutter"` | Hiển thị sách chứa "Flutter" |
| | Có (tên tác giả) | `"Nguyễn"` | Hiển thị sách của tác giả Nguyễn |
| | Không | `"XYZ123"` | Danh sách rỗng |
| Phân biệt HOA/thường? | Chữ thường | `"flutter"` | Kết quả giống "Flutter" |
| | Chữ HOA | `"FLUTTER"` | Kết quả giống "Flutter" |

### IDM — Mượn sách (REQ-04, REQ-05)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Trạng thái sách? | Có sẵn | BOOK001 | Cho phép mượn |
| | Đang mượn | BOOK003 | Không cho phép |
| | Thất lạc | BOOK007 | Không cho phép |
| Trạng thái thành viên? | Hoạt động | MEM002 | Cho phép mượn |
| | Tạm ngưng | MEM004 | Từ chối, thông báo lỗi |
| | Hết hạn | MEM005 | Từ chối, thông báo lỗi |
| Số sách đang mượn? | < 3 (BVA: 0, 1, 2) | MEM006 (0 sách) | Cho phép mượn |
| | = 3 (BVA: giới hạn) | MEM đã mượn 3 sách | Từ chối, thông báo vượt giới hạn |

### IDM — Quy trình Trả sách & Xử lý Quá hạn (REQ-05, REQ-06)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Hạn trả thực tế (REQ-05) | Đúng hạn (Ngày trả $\le$ Hạn trả) | Phiếu mượn còn hạn | Sách chuyển về trạng thái "Có sẵn", hệ thống đóng phiếu mượn. |
| | Quá hạn (Ngày trả $>$ Hạn trả) | Phiếu mượn trễ hạn | Sách về trạng thái "Có sẵn", hệ thống đưa ra cảnh báo vi phạm quá hạn. |
| Cơ chế quét dữ liệu (REQ-06) | Tồn tại phiếu có Ngày hết hạn $\le$ Ngày hiện tại | Kích hoạt nút "Kiểm tra quá hạn" | Tự động chuyển đổi trạng thái các phiếu mượn hết hạn sang "Quá hạn". |
| Phân quyền xem phiếu quá hạn (REQ-06) | Vai trò Thủ thư | `librarian@library.com` | Hiển thị toàn bộ danh sách các phiếu mượn quá hạn trong hệ thống. |
| | Vai trò Thành viên | `ba.nguyen@email.com` | Chỉ hiển thị các phiếu quá hạn thuộc sở hữu của chính độc giả đó. |

### IDM — Quản lý Thành viên & Tra cứu Phiếu mượn (REQ-07, REQ-08)

| Đặc tính (Characteristic) | Phân vùng (Block) | Giá trị đại diện (Value) | Kết quả mong đợi |
|---|---|---|---|
| Định dạng Email mới (REQ-07) | Cấu trúc hợp lệ | `quang26@gmail.com` | Hệ thống phê duyệt form, tạo tài khoản thành công. |
| | Sai định dạng email | `quang@gmail` | Hệ thống báo lỗi định dạng ngay tại trường nhập (Validation error). |
| Trạng thái Email trong DB (REQ-07) | Chưa tồn tại | `newuser@gmail.com` | Khởi tạo thành viên mới thành công. |
| | Đã tồn tại | `ba.nguyen@email.com` | Hệ thống từ chối lưu, hiển thị thông báo trùng lặp dữ liệu. |
| Thao tác tra cứu bản ghi (REQ-08) | Quyền Thủ thư | `librarian@library.com` | Xem được chi tiết toàn bộ phiếu mượn của mọi thành viên. |
| | Quyền Thành viên (Truy cập bản ghi cá nhân) | `ba.nguyen@email.com` | Hiển thị chi tiết bản ghi mượn sách của bản thân. |
| | Quyền Thành viên (Truy cập bản ghi người khác) | Truy cập ID phiếu `BR002` | Hệ thống từ chối truy cập, hiển thị thông báo lỗi phân quyền. |

> 💡 **Gợi ý kỹ thuật**: Sử dụng **Phân lớp tương đương (EP)** cho các phân vùng rời rạc, **Phân tích giá trị biên (BVA)** cho các phân vùng số (ví dụ: giới hạn 3 sách). Xem textbook §6.1–6.3.

---

## Bước 2: Test Cases

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|---|---|---|---|---|---|---|---|
| TC-01 | Đăng nhập thành công với quyền Thủ thư | Trang đăng nhập đã hiển thị | 1. Nhập Email hợp lệ của Thủ thư.<br>2. Nhập Mật khẩu chính xác.<br>3. Nhấn nút Đăng nhập. | Email: [Điền email thủ thư thật]<br>Mật khẩu: [Điền mật khẩu thật] | Đăng nhập thành công. Hệ thống chuyển hướng vào trang quản trị và hiển thị đầy đủ các Tab chức năng. | REQ-01 | EP |
| TC-02 | Đăng nhập thất bại do Email không tồn tại | Trang đăng nhập đã hiển thị | 1. Nhập một Email chưa từng đăng ký.<br>2. Nhập mật khẩu bất kỳ.<br>3. Nhấn nút Đăng nhập. | Email: [Điền một email không tồn tại]<br>Mật khẩu: [Điền mật khẩu bất kỳ] | Hệ thống chặn đăng nhập và hiển thị thông báo lỗi: "[Điền câu báo lỗi thực tế trên web]" | REQ-01 | EP |
| TC-03 | Đăng nhập thất bại do sai mật khẩu | Trang đăng nhập đã hiển thị | 1. Nhập Email hợp lệ của Thủ thư.<br>2. Nhập Mật khẩu sai.<br>3. Nhấn nút Đăng nhập. | Email: [Điền email thủ thư thật]<br>Mật khẩu: [Điền một mật khẩu sai] | Hệ thống chặn đăng nhập và hiển thị thông báo lỗi: "[Điền câu báo lỗi thực tế trên web]" | REQ-01 | EP |
| TC-04 | Chặn đăng nhập khi để trống trường thông tin | Trang đăng nhập đã hiển thị | 1. Để trống cả trường Email và Mật khẩu.<br>2. Nhấn nút Đăng nhập. | Email: ""<br>Mật khẩu: "" | Hệ thống ngăn gửi form và hiển thị cảnh báo: "[Điền câu cảnh báo thực tế của trình duyệt hoặc web]" | REQ-01 | EP |
| TC-05 | Hiển thị toàn bộ danh sách sách mặc định | Đã đăng nhập thành công vào hệ thống | 1. Di chuyển và click chọn Tab Sách.<br>2. Quan sát giao diện danh mục sách. | Trạng thái: Mặc định (Không nhập từ khóa) | Hệ thống tải và hiển thị đầy đủ danh sách sách bao gồm các thông tin thuộc thuộc đặc tả REQ-02. | REQ-02 | EP |
| TC-06 | Tìm kiếm sách chính xác theo tiêu đề | Đang ở giao diện danh mục Sách | 1. Nhập một tên sách chính xác đang có trên web vào ô tìm kiếm.<br>2. Nhấn phím Enter (hoặc nút Tìm). | Từ khóa: "[Điền tên sách thật có trên web]" | Danh sách cập nhật và chỉ hiển thị các cuốn sách có tiêu đề chứa từ khóa đã nhập. | REQ-03 | EP |
| TC-07 | Tìm kiếm sách chính xác theo tên tác giả | Đang ở giao diện danh mục Sách | 1. Nhập tên một tác giả có trong hệ thống vào ô tìm kiếm.<br>2. Nhấn phím Enter (hoặc nút Tìm). | Từ khóa: "[Điền tên tác giả thật có trên web]" | Danh sách cập nhật và hiển thị toàn bộ các đầu sách được viết bởi tác giả đó. | REQ-03 | EP |
| TC-08 | Tìm kiếm sách với từ khóa không tồn tại | Đang ở giao diện danh mục Sách | 1. Nhập một chuỗi ký tự ngẫu nhiên không có trong hệ thống vào ô tìm kiếm.<br>2. Nhấn phím Enter. | Từ khóa: "[Điền chuỗi ký tự lạ bất kỳ]" | Danh sách kết quả trống rỗng, hệ thống hiển thị dòng thông báo: "[Điền câu báo không tìm thấy thực tế trên web]" | REQ-03 | EP, BVA |
| TC-09 | Tìm kiếm sách không phân biệt chữ hoa và chữ thường | Đang ở giao diện danh mục Sách | 1. Nhập từ khóa tên sách bằng kiểu viết chữ hoa thường xen kẽ.<br>2. Nhấn phím Enter. | Từ khóa: [Điền tên sách viết cách điệu hoa thường] | Hệ thống trả về kết quả chính xác tương đương với khi tìm kiếm bằng từ khóa viết thường hoặc viết hoa chuẩn. | REQ-03 | EP |
| TC-10 | Mượn sách thành công với sách có sẵn và thành viên hoạt động | Đã đăng nhập tài khoản Thủ thư, mở tab Mượn sách | 1. Chọn một thành viên đang trạng thái hoạt động.<br>2. Chọn một cuốn sách đang có sẵn.<br>3. Nhấn nút Xác nhận mượn. | Mã sách: [Mã sách sẵn thật]<br>Email độc giả: [Email thành viên hoạt động thật] | Hệ thống báo mượn sách thành công. Trạng thái cuốn sách lập tức chuyển sang "Đã mượn" (hoặc trạng thái tương đương trên web) và tạo mới 1 bản ghi phiếu mượn. | REQ-04 | EP |
| TC-11 | Chặn mượn sách đối với cuốn sách đã được mượn | Đã đăng nhập tài khoản Thủ thư, mở tab Mượn sách | 1. Chọn một thành viên hợp lệ.<br>2. Chọn một cuốn sách có trạng thái đang mượn.<br>3. Kiểm tra tính tương tác của nút hoặc thực hiện mượn. | Mã sách: [Mã sách đang mượn thật]<br>Email độc giả: [Email thành viên hợp lệ] | Hệ thống từ chối xử lý, nút mượn bị vô hiệu hóa hoặc hiển thị thông báo lỗi: "[Điền thông báo lỗi thực tế nếu có]" | REQ-04 | EP |
| TC-12 | Chặn mượn sách đối với sách bị thất lạc | Đã đăng nhập tài khoản Thủ thư, mở tab Mượn sách | 1. Chọn một thành viên hợp lệ.<br>2. Chọn một cuốn sách có trạng thái thất lạc.<br>3. Kiểm tra tính tương tác hoặc thực hiện mượn. | Mã sách: [Mã sách thất lạc thật]<br>Email độc giả: [Email thành viên hợp lệ] | Hệ thống từ chối cho mượn, hiển thị thông báo lỗi hoặc vô hiệu hóa hành động: "[Điền thực tế hiển thị trên web]" | REQ-04 | EP |
| TC-13 | Chặn thành viên có trạng thái "Tạm ngưng" mượn sách | Đã đăng nhập tài khoản Thủ thư, mở tab Mượn sách | 1. Chọn mã thành viên bị tạm ngưng.<br>2. Chọn một cuốn sách có sẵn.<br>3. Thực hiện nhấn nút Xác nhận mượn. | Mã sách: [Mã sách sẵn thật]<br>Email độc giả: [Email thành viên tạm ngưng thật] | Hệ thống từ chối yêu cầu và xuất hiện cảnh báo lỗi: "[Điền câu thông báo lỗi thực tế trên web]" | REQ-04 | EP |
| TC-14 | Chặn thành viên có trạng thái "Hết hạn" mượn sách | Đã đăng nhập tài khoản Thủ thư, mở tab Mượn sách | 1. Chọn mã thành viên đã hết hạn tài khoản.<br>2. Chọn một cuốn sách có sẵn.<br>3. Thực hiện nhấn nút Xác nhận mượn. | Mã sách: [Mã sách sẵn thật]<br>Email độc giả: [Email thành viên hết hạn thật] | Hệ thống từ chối yêu cầu và xuất hiện cảnh báo lỗi: "[Điền câu thông báo lỗi thực tế trên web]" | REQ-04 | EP |
| TC-15 | Chặn mượn sách khi thành viên chạm mốc giới hạn (3 cuốn) | Đã đăng nhập tài khoản Thủ thư, mở tab Mượn sách | 1. Chọn mã thành viên đã mượn sẵn đủ 3 cuốn sách chưa trả.<br>2. Chọn thêm một cuốn sách có sẵn khác.<br>3. Thực hiện nhấn nút Xác nhận mượn. | Mã sách: [Mã sách sẵn thật]<br>Email độc giả: [Email thành viên giữ 3 sách thật] | Hệ thống chặn thao tác và hiển thị lỗi: "[Điền câu thông báo vượt giới hạn thực tế trên web]" | REQ-04 | BVA |
| TC-16 | Đóng phiếu mượn thành công khi độc giả trả sách đúng hạn | Đã đăng nhập tài khoản Thủ thư, mở tab Mượn/Trả | 1. Tìm kiếm phiếu mượn còn trong hạn.<br>2. Nhấn nút Trả sách tại phiếu đó. | Tình trạng phiếu: Còn hạn (Ngày trả $\le$ Hạn trả)<br>Mã phiếu: [Mã phiếu còn hạn thật] | Hệ thống ghi nhận trả sách thành công. Trạng thái cuốn sách tự động cập nhật về trạng thái có sẵn để mượn tiếp theo đặc tả. | REQ-05 | EP |
| TC-17 | Hiển thị thông báo cảnh báo khi độc giả trả sách quá hạn | Đã đăng nhập tài khoản Thủ thư, mở tab Mượn/Trả | 1. Tìm kiếm phiếu mượn đã trễ hạn.<br>2. Nhấn nút Trả sách tại phiếu đó. | Tình trạng phiếu: Quá hạn (Ngày trả $>$ Hạn trả)<br>Mã phiếu: [Mã phiếu trễ hạn thật] | Ghi nhận trả sách thành công, đưa sách về trạng thái sẵn sàng và hiển thị cảnh báo: "[Điền nội dung cảnh báo trễ hạn trên web]" | REQ-05 | EP |
| TC-18 | Kích hoạt quét hệ thống để xử lý và cập nhật phiếu quá hạn | Đã đăng nhập với quyền Thủ thư | 1. Di chuyển sang màn hình quản lý phiếu.<br>2. Nhấn nút lệnh Kiểm tra quá hạn. | Hành động: Click nút chức năng hệ thống | Hệ thống tự động thực hiện quét dữ liệu, đổi nhãn tất cả các phiếu có Ngày hết hạn $\le$ ngày hiện tại sang trạng thái "Quá hạn". | REQ-06 | EP |
| TC-19 | Phân quyền hiển thị danh sách phiếu quá hạn của Thủ thư | Đã đăng nhập với quyền Thủ thư | 1. Truy cập vào giao diện tổng hợp phiếu quá hạn.<br>2. Quan sát phạm vi số lượng bản ghi hiển thị. | Vai trò: [Điền email thủ thư thật] | Giao diện hiển thị đầy đủ toàn bộ danh sách các phiếu mượn quá hạn của tất cả các thành viên trong toàn bộ thư viện. | REQ-06 | EP |
| TC-20 | Phân quyền hiển thị danh sách phiếu quá hạn của Thành viên | Đã đăng nhập với quyền Độc giả | 1. Truy cập vào giao diện quản lý tài khoản cá nhân.<br>2. Quan sát mục phiếu quá hạn hiển thị. | Vai trò: [Điền email thành viên thật] | Giao diện chỉ hiển thị duy nhất các bản ghi phiếu quá hạn thuộc quyền sở hữu cá nhân của chính tài khoản đang đăng nhập. | REQ-06 | EP |
| TC-21 | Tạo thành viên mới thành công với các thông tin hợp lệ | Đã đăng nhập với quyền Thủ thư | 1. Vào mục quản lý thành viên, chọn Thêm mới.<br>2. Nhập đầy đủ Họ tên, Email hợp lệ và SĐT.<br>3. Nhấn nút Xác nhận (hoặc nút Lưu). | Tên: [Họ tên hợp lệ]<br>Email: [Email hợp lệ chưa tồn tại]<br>SĐT: [SĐT hợp lệ] | Hệ thống xử lý dữ liệu form thành công, tạo tài khoản mới và xuất hiện thông báo: "[Điền câu thông báo thành công thực tế trên web]" | REQ-07 | EP |
| TC-22 | Chặn khởi tạo tài khoản do Email sai định dạng cấu trúc | Đã đăng nhập với quyền Thủ thư | 1. Vào mục thêm mới thành viên.<br>2. Nhập Email thiếu các ký tự định dạng cấu trúc chuẩn (thiếu @ hoặc dấu chấm domain).<br>3. Nhấn nút Xác nhận. | Tên: [Họ tên hợp lệ]<br>Email: [Điền email sai định dạng]<br>SĐT: [SĐT hợp lệ] | Hệ thống chặn tiến trình gửi dữ liệu, xuất hiện thông báo cảnh báo lỗi hiển thị rõ: "[Điền câu báo lỗi email thực tế trên web]" | REQ-07 | EP |
| TC-23 | Chặn khởi tạo tài khoản do trùng lặp Email trong cơ sở dữ liệu | Đã đăng nhập với quyền Thủ thư | 1. Vào mục thêm mới thành viên.<br>2. Nhập một địa chỉ Email đã được đăng ký trước đó trên hệ thống.<br>3. Nhấn nút Xác nhận. | Tên: [Họ tên hợp lệ]<br>Email: [Điền email đã có sẵn trong file dữ liệu thật]<br>SĐT: [SĐT hợp lệ] | Hệ thống từ chối lưu dữ liệu, xuất hiện thông báo lỗi hiển thị rõ: "[Điền câu thông báo trùng email thực tế trên web]" | REQ-07 | EP |
| TC-24 | Kiểm tra quyền hạn tra cứu phiếu mượn của tài khoản Thủ thư | Đã đăng nhập với quyền Thủ thư | 1. Truy cập vào ô tìm kiếm lịch sử mượn trả sách.<br>2. Nhập thông tin tra cứu phiếu mượn của một thành viên bất kỳ. | Vai trò: [Điền email thủ thư thật] | Hệ thống cho phép hiển thị chi tiết toàn bộ thông tin lịch sử bản ghi mượn sách của mọi độc giả. | REQ-08 | EP |
| TC-25 | Chặn Thành viên thực hiện xem chi tiết phiếu mượn của người khác | Đã đăng nhập với quyền Độc giả | 1. Truy cập vào thanh tra cứu nhanh hoặc đường dẫn phiếu mượn.<br>2. Nhập mã phiếu mượn không thuộc sở hữu cá nhân. | Mã phiếu: [Điền mã phiếu của tài khoản khác] | Hệ thống chặn hiển thị dữ liệu chi tiết, trả về thông báo lỗi phân quyền hoặc thông báo: "[Điền thông báo chặn thực tế trên web]" | REQ-08 | EP |
---

## Tổng hợp

| Nhóm chức năng | Số TC | REQ phủ | Kỹ thuật IDM áp dụng |
|----------------|-------|---------|----------------------|
| | | | |
| **Tổng** | **<!-- ≥ 20 -->** | | |
