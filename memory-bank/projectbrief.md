# Project Brief: Share Mouse with Phone

## Mục tiêu dự án

Phát triển một giải pháp phần mềm cho phép người dùng chia sẻ chuột và bàn phím giữa máy tính Windows và điện thoại Android, tạo ra trải nghiệm liền mạch khi làm việc trên nhiều thiết bị.

## Phạm vi dự án

### Trong phạm vi

1. **Ứng dụng Windows (Server)**
   - Bắt sự kiện chuột và bàn phím toàn hệ thống
   - Phát hiện khi chuột di chuyển qua ranh giới màn hình
   - Gửi dữ liệu sự kiện đến thiết bị Android qua mạng
   - Giao diện người dùng để cấu hình kết nối và ranh giới màn hình

2. **Ứng dụng Android (Client)**
   - Kết nối với ứng dụng Windows qua mạng
   - Nhận và xử lý dữ liệu sự kiện chuột/bàn phím
   - Mô phỏng các thao tác chạm và nhập liệu trên Android
   - Hiển thị con trỏ ảo trên màn hình Android

3. **Giao thức truyền thông**
   - Định nghĩa giao thức truyền dữ liệu giữa PC và Android
   - Tối ưu hóa để giảm độ trễ
   - Đảm bảo tính bảo mật của kết nối

### Ngoài phạm vi

1. Hỗ trợ các hệ điều hành khác ngoài Windows và Android
2. Chia sẻ clipboard giữa các thiết bị (có thể bổ sung trong tương lai)
3. Chia sẻ file giữa các thiết bị (có thể bổ sung trong tương lai)
4. Hỗ trợ kết nối qua Bluetooth (chỉ hỗ trợ kết nối Wi-Fi/LAN)
5. Các giải pháp yêu cầu root trên Android

## Yêu cầu chức năng

1. Người dùng có thể di chuyển chuột liền mạch từ màn hình PC sang màn hình Android
2. Người dùng có thể sử dụng bàn phím PC để nhập liệu trên Android
3. Người dùng có thể cấu hình vị trí tương đối của thiết bị Android so với màn hình PC
4. Ứng dụng phải hoạt động mà không cần quyền root trên Android
5. Kết nối phải được bảo mật để ngăn chặn các bên thứ ba can thiệp
6. Độ trễ phải được giảm thiểu để đảm bảo trải nghiệm người dùng tốt

## Công nghệ dự kiến

### Phía Windows
- C# với .NET Framework
- Windows API (User32.dll) cho các hook chuột và bàn phím
- Socket TCP/IP cho giao tiếp mạng

### Phía Android
- Java/Kotlin
- Android Accessibility Service API
- Socket TCP/IP cho giao tiếp mạng

## Các bên liên quan

- Người dùng cuối: Những người làm việc với cả PC và điện thoại Android
- Nhà phát triển: Đội ngũ phát triển dự án

## Tiêu chí thành công

1. Ứng dụng hoạt động ổn định trên Windows 10+ và Android 8.0+
2. Độ trễ giữa thao tác trên PC và phản hồi trên Android dưới 100ms
3. Tỷ lệ chính xác của các thao tác chuột và bàn phím trên Android đạt trên 95%
4. Giao diện người dùng trực quan và dễ sử dụng
5. Không yêu cầu quyền root trên Android