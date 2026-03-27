# Hotel Booking System Testing Project

> Manual Testing & Database Verification

## Tổng quan dự án

Dự án tập trung vào việc lập kế hoạch và thực thi kiểm thử cho hệ thống đặt phòng khách sạn trực tuyến, bao gồm luồng nghiệp vụ từ tìm kiếm đến thanh toán và xác minh dữ liệu tầng Database.

| Mục tiêu | Chi tiết |
|---|---|
| **Hệ thống kiểm thử** | Nền tảng đặt phòng khách sạn trực tuyến |
| **Phạm vi** | Tìm kiếm, Đặt phòng, Thanh toán & Database |
| **Loại kiểm thử** | Manual Testing, Black-box, Database Testing |
| **Công cụ** | Excel, SQL, Postman |

---

## Tài liệu dự án (Project Artifacts)

Hệ thống tài liệu được tổ chức để minh họa quy trình kiểm thử từ phân tích đến thiết kế:

* **[Phân tích yêu cầu](./docs/RequirementsAnalysis.md)**:  Chi tiết các yêu cầu chức năng, phi chức năng và căn cứ thiết kế kịch bản kiểm thử.
* **[Danh sách Test Case (Markdown Version)](./docs/TestCases.md)**: Bản xem nhanh 96 kịch bản kiểm thử trực tiếp trên trình duyệt.
* **[Danh sách Test Case (Excel Version)](./resources/QC_Intern_TestCase_TranPhamTrucAnh.xlsx)**: File đầy đủ các kịch bản kiểm thử chức năng và Database.

---
## Kỹ năng thể hiện

* **Phân tích yêu cầu nghiệp vụ từ hệ thống thực tế:** Khả năng bóc tách luồng nghiệp vụ từ tài liệu đặc tả.
* **Tư duy kiểm thử Database:** Xác minh tính toàn vẹn dữ liệu, kiểm tra các ràng buộc (Constraints), xử lý giao dịch đồng thời (Concurrency) và cơ chế lưu trữ (Soft Delete).
* **Xử lý tình huống ngoại lệ:** Thiết kế kịch bản cho các lỗi hệ thống như Timeout cổng thanh toán, lỗi kết nối mạng và xung đột dữ liệu (Overbooking).
