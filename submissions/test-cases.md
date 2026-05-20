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

<!-- Tự tổ chức bảng test case: có thể chia nhóm theo chức năng, theo REQ, hoặc theo luồng nghiệp vụ — tùy nhóm quyết định. -->
<!-- Mỗi TC phải ánh xạ ngược về ít nhất 1 dòng trong bảng IDM ở Bước 1. -->

| Mã TC | Mục tiêu kiểm thử | Tiền điều kiện | Bước thực hiện | Dữ liệu đầu vào | Kết quả mong đợi | REQ | Kỹ thuật |
|-------|-------------------|---------------|---------------|-----------------|------------------|-----|---------|
| | | | | | | | |

---

## Tổng hợp

| Nhóm chức năng | Số TC | REQ phủ | Kỹ thuật IDM áp dụng |
|----------------|-------|---------|----------------------|
| | | | |
| **Tổng** | **<!-- ≥ 20 -->** | | |
