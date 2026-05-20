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
| TC-01 | Đăng nhập thành công với quyền Thủ thư | Trang đăng nhập đã hiển thị | 1. Nhập Email hợp lệ của Thủ thư. 2. Nhập Mật khẩu chính xác. 3. Nhấn nút Đăng nhập. | Email: librarian@library.com, Mật khẩu: admin123 | Đăng nhập thành công. Hệ thống chuyển hướng vào trang quản trị và hiển thị đầy đủ các Tab chức năng. | REQ-01 | EP |
| TC-02 | Đăng nhập thất bại do Email không tồn tại | Trang đăng nhập đã hiển thị | 1. Nhập Email chưa đăng ký. 2. Nhập mật khẩu bất kỳ. 3. Nhấn nút Đăng nhập. | Email: noone@email.com, Mật khẩu: admin123 | Hệ thống chặn đăng nhập và hiển thị thông báo lỗi: "Tài khoản không tồn tại". | REQ-01 | EP |
| TC-03 | Đăng nhập thất bại do sai mật khẩu | Trang đăng nhập đã hiển thị | 1. Nhập Email hợp lệ. 2. Nhập Mật khẩu sai. 3. Nhấn nút Đăng nhập. | Email: librarian@library.com, Mật khẩu: wrongpass | Hệ thống chặn đăng nhập và hiển thị thông báo lỗi: "Mật khẩu không đúng". | REQ-01 | EP |
| TC-04 | Chặn đăng nhập khi để trống trường thông tin | Trang đăng nhập đã hiển thị | 1. Để trống trường Email và Mật khẩu. 2. Nhấn nút Đăng nhập. | Email: "", Mật khẩu: "" | Hệ thống ngăn gửi form và hiển thị cảnh báo: "Vui lòng nhập đầy đủ thông tin". | REQ-01 | EP |
| TC-05 | Hiển thị toàn bộ danh sách sách mặc định | Đã đăng nhập thành công vào hệ thống | 1. Di chuyển và click chọn Tab Sách. 2. Quan sát giao diện danh mục sách. | Trạng thái: Mặc định (Không nhập từ khóa) | Hệ thống tải và hiển thị đầy đủ danh sách sách bao gồm: Tên sách, tác giả, thể loại và trạng thái hiện tại. | REQ-02 | EP |
| TC-06 | Tìm kiếm sách chính xác theo tiêu đề | Đang ở giao diện danh mục Sách | 1. Nhập tên sách có tồn tại vào ô tìm kiếm. 2. Nhấn phím Enter. | Từ khóa: "Flutter" | Danh sách cập nhật và chỉ hiển thị các cuốn sách có tiêu đề chứa từ khóa "Flutter". | REQ-03 | EP |
| TC-07 | Tìm kiếm sách chính xác theo tên tác giả | Đang ở giao diện danh mục Sách | 1. Nhập tên tác giả có trong hệ thống vào ô tìm kiếm. 2. Nhấn phím Enter. | Từ khóa: "Nguyễn" | Danh sách cập nhật và hiển thị toàn bộ các đầu sách được viết bởi tác giả có tên chứa chữ "Nguyễn". | REQ-03 | EP |
| TC-08 | Tìm kiếm sách với từ khóa không tồn tại | Đang ở giao diện danh mục Sách | 1. Nhập một chuỗi ký tự lạ vào ô tìm kiếm. 2. Nhấn phím Enter. | Từ khóa: "XYZ123" | Danh sách kết quả trống rỗng, hệ thống hiển thị dòng thông báo: "Không tìm thấy sách phù hợp". | REQ-03 | EP, BVA |
| TC-09 | Tìm kiếm sách không phân biệt chữ hoa và chữ thường | Đang ở giao diện danh mục Sách | 1. Nhập từ khóa dạng chữ thường xen kẽ chữ hoa. 2. Nhấn phím Enter. | Từ khóa: "fLuTtEr" | Hệ thống trả về kết quả chính xác giống như khi tìm kiếm bằng từ khóa viết chuẩn "Flutter". | REQ-03 | EP |
| TC-10 | Mượn sách thành công với sách có sẵn và thành viên hoạt động | Đã đăng nhập tài khoản Thủ thư, mở tab Mượn sách | 1. Chọn thành viên đang trạng thái hoạt động. 2. Chọn cuốn sách đang có sẵn. 3. Nhấn nút Xác nhận mượn. | Mã sách: BK01, Email độc giả: ba.nguyen@email.com | Hệ thống báo mượn sách thành công. Trạng thái cuốn sách lập tức chuyển sang "Đã mượn", tạo mới 1 bản ghi phiếu mượn. | REQ-04 | EP |
| TC-11 | Chặn mượn sách đối với cuốn sách đã được mượn | Đã đăng nhập tài khoản Thủ thư, mở tab Mượn sách | 1. Chọn thành viên hợp lệ. 2. Chọn cuốn sách có trạng thái đang mượn. 3. Nhấn nút Xác nhận mượn. | Mã sách: BK03, Email độc giả: ba.nguyen@email.com | Hệ thống từ chối xử lý, nút mượn bị vô hiệu hóa hoặc hiển thị thông báo: "Sách hiện không có sẵn". | REQ-04 | EP |
| TC-12 | Chặn mượn sách đối với sách bị thất lạc | Đã đăng nhập tài khoản Thủ thư, mở tab Mượn sách | 1. Chọn thành viên hợp lệ. 2. Chọn cuốn sách có trạng thái thất lạc. 3. Nhấn nút Xác nhận mượn. | Mã sách: BK07, Email độc giả: ba.nguyen@email.com | Hệ thống từ chối cho mượn và hiển thị lỗi thông báo: "Sách đã bị thất lạc, không thể mượn". | REQ-04 | EP |
| TC-13 | Chặn thành viên có trạng thái "Tạm ngưng" mượn sách | Đã đăng nhập tài khoản Thủ thư, mở tab Mượn sách | 1. Chọn mã thành viên bị tạm ngưng. 2. Chọn một cuốn sách có sẵn. 3. Nhấn nút Xác nhận mượn. | Mã sách: BK01, Email độc giả: cu.le@email.com | Hệ thống từ chối yêu cầu và xuất hiện cảnh báo lỗi: "Tài khoản đang bị tạm ngưng hoạt động". | REQ-04 | EP |
| TC-14 | Chặn thành viên có trạng thái "Hết hạn" mượn sách | Đã đăng nhập tài khoản Thủ thư, mở tab Mượn sách | 1. Chọn mã thành viên đã hết hạn tài khoản. 2. Chọn một cuốn sách có sẵn. 3. Nhấn nút Xác nhận mượn. | Mã sách: BK01, Email độc giả: binh.pham@email.com | Hệ thống từ chối yêu cầu và xuất hiện cảnh báo lỗi: "Thẻ thành viên đã hết hạn sử dụng". | REQ-04 | EP |
| TC-15 | Chặn mượn sách khi thành viên chạm mốc giới hạn (3 cuốn) | Đã đăng nhập tài khoản Thủ thư, mở tab Mượn sách | 1. Chọn mã thành viên đã mượn sẵn 3 cuốn chưa trả. 2. Chọn một cuốn sách có sẵn khác. 3. Nhấn nút Xác nhận mượn. | Mã sách: BK01, Email độc giả: an.tran@email.com (đã giữ sẵn 3 sách) | Hệ thống chặn thao tác và hiển thị lỗi: "Thành viên đã vượt quá số lượng sách được mượn tối đa (3 cuốn)". | REQ-04 | BVA |
| TC-16 | Đóng phiếu mượn thành công khi độc giả trả sách đúng hạn | Đã đăng nhập tài khoản Thủ thư, mở tab Mượn/Trả | 1. Tìm kiếm phiếu mượn còn trong hạn. 2. Nhấn nút Trả sách tại phiếu đó. | Tình trạng phiếu: Còn hạn (Ngày trả <= Hạn trả), Mã phiếu: BR-OK-01 | Hệ thống ghi nhận trả sách thành công. Trạng thái cuốn sách tự động cập nhật về nhãn màu xanh "Có sẵn". | REQ-05 | EP |
| TC-17 | Hiển thị thông báo cảnh báo khi độc giả trả sách quá hạn | Đã đăng nhập tài khoản Thủ thư, mở tab Mượn/Trả | 1. Tìm kiếm phiếu mượn đã trễ hạn. 2. Nhấn nút Trả sách tại phiếu đó. | Tình trạng phiếu: Quá hạn (Ngày trả > Hạn trả), Mã phiếu: BR-OD-01 | Ghi nhận trả sách thành công, đưa sách về trạng thái "Có sẵn" và hiển thị cảnh báo: "Sách trả quá hạn quy định". | REQ-05 | EP |
| TC-18 | Kích hoạt quét hệ thống để xử lý và cập nhật phiếu quá hạn | Đã đăng nhập với quyền Thủ thư | 1. Di chuyển sang màn hình quản lý phiếu. 2. Nhấn nút lệnh Kiểm tra quá hạn. | Hành động: Kích hoạt nút bấm | Hệ thống tự động quét dữ liệu, đổi nhãn tất cả các phiếu có Ngày hết hạn <= ngày hiện tại sang trạng thái "Quá hạn". | REQ-06 | EP |
| TC-19 | Phân quyền hiển thị danh sách phiếu quá hạn của Thủ thư | Đã đăng nhập với quyền Thủ thư | 1. Truy cập vào giao diện tổng hợp phiếu quá hạn. 2. Quan sát số lượng bản ghi hiển thị. | Vai trò: librarian@library.com | Giao diện hiển thị đầy đủ toàn bộ danh sách các phiếu mượn quá hạn của tất cả các thành viên trong thư viện. | REQ-06 | EP |
| TC-20 | Phân quyền hiển thị danh sách phiếu quá hạn của Thành viên | Đã đăng nhập với quyền Độc giả | 1. Truy cập vào giao diện quản lý tài khoản cá nhân. 2. Quan sát mục phiếu quá hạn. | Vai trò: ba.nguyen@email.com | Giao diện chỉ hiển thị duy nhất các bản ghi phiếu quá hạn thuộc quyền sở hữu của chính tài khoản này. | REQ-06 | EP |
| TC-21 | Tạo thành viên mới thành công với các thông tin hợp lệ | Đã đăng nhập với quyền Thủ thư | 1. Vào mục quản lý thành viên, chọn Thêm mới. 2. Nhập đầy đủ Họ tên, Email hợp lệ và SĐT. 3. Nhấn nút Xác nhận. | Tên: Nguyễn Huy Quang, Email: quang26@gmail.com, SĐT: 0912345678 | Hệ thống xử lý form thành công, tạo tài khoản mới và xuất hiện thông báo: "Thêm thành viên thành công". | REQ-07 | EP |
| TC-22 | Chặn khởi tạo tài khoản do Email sai định dạng cấu trúc | Đã đăng nhập với quyền Thủ thư | 1. Vào mục thêm mới thành viên. 2. Nhập Email thiếu dấu chấm phân tách phần domain. 3. Nhấn nút Xác nhận. | Tên: Nguyễn Huy Quang, Email: quang@gmail, SĐT: 0912345678 | Hệ thống chặn tiến trình gửi dữ liệu, xuất hiện thông báo cảnh báo tại trường nhập: "Email không hợp lệ". | REQ-07 | EP |
| TC-23 | Chặn khởi tạo tài khoản do trùng lặp Email trong cơ sở dữ liệu | Đã đăng nhập với quyền Thủ thư | 1. Vào mục thêm mới thành viên. 2. Nhập một địa chỉ Email đã được đăng ký trước đó. 3. Nhấn nút Xác nhận. | Tên: Nguyễn Huy Quang, Email: ba.nguyen@email.com, SĐT: 0912345678 | Hệ thống từ chối lưu dữ liệu, xuất hiện thông báo lỗi hiển thị rõ: "Email đã tồn tại". | REQ-07 | EP |
| TC-24 | Kiểm tra quyền hạn tra cứu phiếu mượn của tài khoản Thủ thư | Đã đăng nhập với quyền Thủ thư | 1. Truy cập vào ô tìm kiếm lịch sử mượn. 2. Gõ tìm kiếm thông tin phiếu bất kỳ. | Vai trò: librarian@library.com | Hệ thống cho phép hiển thị chi tiết thông tin gồm mã phiếu, sách mượn, ngày mượn, ngày hết hạn của mọi độc giả. | REQ-08 | EP |
| TC-25 | Chặn Thành viên thực hiện xem chi tiết phiếu mượn của người khác | Đã đăng nhập với quyền Độc giả | 1. Truy cập vào thanh tra cứu nhanh phiếu mượn. 2. Nhập mã phiếu mượn không thuộc sở hữu cá nhân. | Mã phiếu: BR-OK-02 (Phiếu thuộc tài khoản an.tran@email.com) | Hệ thống chặn hiển thị dữ liệu chi tiết, trả về thông báo lỗi phân quyền hoặc: "Không tìm thấy bản ghi phiếu mượn hợp lệ". | REQ-08 | EP |
---

## Tổng hợp

| Nhóm chức năng | Số TC | REQ phủ | Kỹ thuật IDM áp dụng |
|----------------|-------|---------|----------------------|
| | | | |
| **Tổng** | **<!-- ≥ 20 -->** | | |
