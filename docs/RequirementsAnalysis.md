# Đặc tả hệ thống – Đặt phòng khách sạn trực tuyến

## PHẦN 1: ĐẶC TẢ CHỨC NĂNG ĐẶT PHÒNG KHÁCH SẠN

### 1.1 Tổng quan luồng nghiệp vụ

Hệ thống web hỗ trợ đặt phòng khách sạn trực tuyến theo luồng 4 bước:

| Bước | Tên bước | Mô tả |
|---|---|---|
| 1 | Tìm kiếm khách sạn | Người dùng nhập thành phố, ngày nhận/trả phòng, số lượng phòng và khách |
| 2 | Chọn phòng | Hệ thống hiển thị danh sách phòng còn trống phù hợp; người dùng chọn loại phòng |
| 3 | Nhập thông tin & thanh toán | Người dùng điền thông tin cá nhân và chọn phương thức thanh toán, hoàn tất booking |
| 4 | Xác nhận booking | Hệ thống hiển thị mã booking, thông tin khách sạn và thông tin thanh toán |

---

### 1.2 Chức năng Tìm kiếm

Người dùng phải nhập đủ cả 3 field (Thành phố, Ngày nhận/trả phòng, Số lượng phòng & khách) mới có thể thực hiện tìm kiếm. Nếu thiếu bất kỳ field nào → hệ thống hiển thị thông báo lỗi tương ứng, không thực hiện tìm kiếm.

#### 1.2.1 Trường Thành phố

Trường "Thành phố" là ô nhập liệu dạng **Flexible Search** cho phép người dùng gõ chữ tự do. Hệ thống xử lý theo cơ chế:

- Gợi ý địa danh hoặc tên khách sạn khớp với từ khóa đang gõ
- Hỗ trợ nhập một phần tên vẫn trả về kết quả (vd: "nội" → gợi ý "Hà Nội")
- Không phân biệt hoa/thường và dấu tiếng Việt ("ha noi" = "Hà Nội")
- Khoảng trắng đầu/cuối được tự động bỏ qua
- Để trống → hệ thống hiển thị thông báo lỗi, không thực hiện tìm kiếm

#### 1.2.2 Trường Ngày nhận / Trả phòng

- Ngày mặc định: Check-in = ngày hiện tại, Check-out = ngày kế tiếp
- Không cho chọn ngày nhận phòng trong quá khứ
- Không cho chọn ngày trả phòng ≤ ngày nhận phòng (tối thiểu 1 đêm)
- Để trống một trong hai trường → hiển thị thông báo lỗi tương ứng

#### 1.2.3 Trường Số lượng phòng & Số lượng khách

Cả hai field dùng UI bộ đếm nút **[+]** / **[-]**, không cho nhập tay.

| Field | Mặc định | Min | Max |
|---|---|---|---|
| Số phòng | 1 | 1 | 10 |
| Số khách | 1 | 1 | 10 |

- Nút [-] bị disable khi giá trị đang ở mức min
- Nút [+] bị disable khi giá trị đang ở mức max
- Hệ thống validate: Số khách ≥ Số phòng trước khi tìm kiếm

---

### 1.3 Chức năng Chọn phòng

Số lượng phòng và số khách được fix cứng từ bước tìm kiếm, **không cho phép chỉnh lại** ở bước này. Hệ thống tự lọc và chỉ hiển thị các phòng đáp ứng đủ số lượng đã chọn.

Trang chọn phòng hiển thị danh sách các phòng available dưới dạng các section riêng, mỗi section có loại phòng, giá và nút chọn. Người dùng chọn 1 phòng rồi mới sang bước nhập thông tin.

Hệ thống hỗ trợ 3 loại phòng tiêu chuẩn:

| Loại phòng | Sức chứa | Mô tả |
|---|---|---|
| Single | 1 khách | Giường đơn |
| Double | 2 khách | Giường đôi hoặc 2 giường đơn |
| Suite | 2–4 khách | Phòng cao cấp, tiện nghi mở rộng |

Nếu không có phòng nào thỏa điều kiện → hệ thống hiển thị thông báo không có phòng trống.

---

### 1.4 Chức năng Nhập thông tin cá nhân

Không có trang xác nhận riêng trước thanh toán. Thông tin booking (loại phòng, ngày, giá) được hiển thị tóm tắt ngay trên trang nhập thông tin cá nhân và thanh toán.

Form thông tin cá nhân gồm 4 field, tất cả đều bắt buộc:

| Field | Định dạng hợp lệ |
|---|---|
| Họ và tên | Chỉ gồm chữ cái và dấu cách, 2–100 ký tự |
| Số điện thoại | 10–11 chữ số, bắt đầu bằng 0 |
| Số CCCD | Đúng 12 chữ số |
| Email | Đúng định dạng email (có @, có domain hợp lệ) |

**Hành vi từng field:**

**Họ và tên**
- Chỉ chấp nhận chữ cái (bao gồm tiếng Việt có dấu) và dấu cách
- Nhập ký tự số hoặc ký tự đặc biệt → hiển thị thông báo lỗi bên dưới field
- Để trống hoặc chỉ nhập dấu cách → lỗi "Vui lòng nhập họ và tên"
- Khoảng trắng đầu/cuối được tự động trim trước khi validate

**Số điện thoại**
- Chỉ chấp nhận ký tự số; ký tự khác bị chặn tại input, không hiển thị
- Phải bắt đầu bằng "0"; nếu không → lỗi sau khi rời khỏi field
- Độ dài hợp lệ: 10–11 chữ số

**Số CCCD**
- Chỉ chấp nhận ký tự số; ký tự khác bị chặn tại input
- Độ dài hợp lệ: đúng 12 chữ số

**Email**
- Validate sau khi rời khỏi field
- Phải có đúng 1 ký tự "@" và domain hợp lệ (vd: gmail.com)
- Không được chứa khoảng trắng
- Sai định dạng → lỗi "Email không hợp lệ"

---

### 1.5 Chức năng Thanh toán

Hệ thống hỗ trợ 2 phương thức thanh toán:

| Phương thức | Ghi chú |
|---|---|
| Thẻ tín dụng / ghi nợ | Visa, Mastercard — nhập số thẻ, ngày hết hạn, CVV |
| Ví điện tử (MoMo / ZaloPay) | Xác thực qua QR code bằng app tương ứng |

**Thẻ tín dụng / ghi nợ:**

| Field | Ràng buộc |
|---|---|
| Số thẻ | Chỉ nhận số; đúng 16 chữ số; để trống → lỗi khi submit |
| Ngày hết hạn (MM/YY) | Không cho chọn tháng/năm đã qua; để trống → lỗi khi submit |
| CVV | Chỉ nhận số; đúng 3 chữ số; để trống → lỗi khi submit |

**Ví điện tử (MoMo / ZaloPay):**
- Sau khi chọn, hệ thống hiển thị mã QR
- Người dùng quét QR bằng app để xác nhận thanh toán
- Nếu không xác nhận trong 5 phút → giao dịch hết hạn, hiển thị thông báo, cho phép thử lại

---

### 1.6 Trang xác nhận Booking

Sau khi hoàn tất thanh toán, hệ thống chuyển sang trang xác nhận booking, hiển thị đầy đủ:

- Mã booking (BookingID)
- Thông tin khách sạn: tên, địa chỉ
- Loại phòng đã đặt
- Ngày nhận phòng và ngày trả phòng
- Tổng tiền thanh toán

---

## PHẦN 2: ĐẶC TẢ CƠ SỞ DỮ LIỆU – THANH TOÁN BOOKING

### 2.1 Cấu trúc bảng Payment

| Trường | Kiểu dữ liệu | Nullable | Mô tả |
|---|---|---|---|
| PaymentID | INT / UUID | NOT NULL | Khóa chính, tự tăng |
| BookingID | INT / UUID | NOT NULL | Khóa ngoại → bảng Booking |
| Amount | DECIMAL(10,2) | NOT NULL | Số tiền thanh toán, > 0 |
| Currency | VARCHAR(3) | NOT NULL | Mã tiền tệ chuẩn ISO 4217 (VND, USD...) |
| PaymentMethod | ENUM / VARCHAR | NOT NULL | CREDIT_CARD \| MOMO \| ZALOPAY |
| PaymentStatus | ENUM | NOT NULL | PENDING \| SUCCESS \| FAILED \| REFUNDED |
| PaymentDate | DATETIME | NOT NULL | Thời điểm thực hiện giao dịch |

---

### 2.2 Quan hệ giữa các bảng



Booking có thể có nhiều Payment records vì:
- **0 record:** user tạo Booking nhưng bỏ ngang, chưa thực hiện thanh toán lần nào
- **Nhiều record:** user thử thanh toán thất bại rồi thử lại, hoặc có phát sinh refund sau đó

**Constraint bổ sung trên bảng Payment:**
- Mỗi BookingID chỉ được phép tồn tại tối đa 1 record có `PaymentStatus = 'SUCCESS'`
- Khi Booking bị hủy sau khi đã thanh toán thành công, hệ thống tạo thêm 1 Payment record mới với `PaymentStatus = 'REFUNDED'`, không sửa record cũ

#### Vòng đời BookingStatus

| Trạng thái | Ý nghĩa |
|---|---|
| PENDING | Booking vừa được tạo, chưa có Payment SUCCESS |
| CONFIRMED | Đã có Payment SUCCESS, phòng được xác nhận |
| CANCELLED | User hoặc hệ thống hủy. Record không bị xóa (soft delete), chỉ cập nhật trạng thái |

---

### 2.3 Cơ chế nhận dạng Customer

Hệ thống không yêu cầu đăng nhập. Email được dùng làm unique key để nhận dạng Customer:

- Email chưa tồn tại trong DB → tạo CustomerID mới
- Email đã tồn tại → reuse CustomerID cũ, không tạo bản ghi trùng
- Cột Email có **UNIQUE constraint** ở tầng database, đảm bảo không thể insert 2 Customer có cùng Email dù từ bất kỳ luồng nào

---

### 2.4 API liên quan đến thanh toán

Hệ thống được triển khai theo kiến trúc RESTful API. Kiểm thử Database thực hiện bằng cách gửi Request qua API và dùng SQL để xác minh.

| API Endpoint | Method | Mô tả chức năng |
|---|---|---|
| /bookings | POST | Tạo một bản ghi Booking mới |
| /bookings/{id}/cancel | PATCH | Hủy đặt phòng (Soft delete — cập nhật status) |
| /payments | POST | Thực hiện giao dịch thanh toán |
