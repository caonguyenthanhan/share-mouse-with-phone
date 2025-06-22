# System Patterns: Share Mouse with Phone

## Kiến trúc hệ thống

Dự án "Share Mouse with Phone" sử dụng mô hình Client-Server, trong đó:

- **Server (Windows)**: Chịu trách nhiệm bắt sự kiện chuột/bàn phím và gửi đến client
- **Client (Android)**: Nhận và mô phỏng các sự kiện chuột/bàn phím từ server

### Sơ đồ kiến trúc tổng thể

```
+------------------+                  +------------------+
|                  |                  |                  |
|  Windows (PC)    |                  |  Android         |
|  Server          |                  |  Client          |
|                  |                  |                  |
+--------+---------+                  +--------+---------+
         |                                     |
         |           TCP/IP Socket             |
         +-------------------------------------+
         |                                     |
+--------+---------+                  +--------+---------+
|                  |                  |                  |
| Mouse/Keyboard   |                  | Accessibility    |
| Hook System      |                  | Service          |
|                  |                  |                  |
+------------------+                  +------------------+
```

## Các thành phần chính

### Phía Windows (Server)

1. **Hook Manager**
   - Sử dụng Windows Hook API để bắt sự kiện chuột và bàn phím toàn hệ thống
   - Xử lý và chuyển đổi sự kiện thành định dạng phù hợp để gửi đi
   - Quyết định khi nào chuyển sự kiện đến Android dựa trên vị trí chuột

2. **Boundary Detector**
   - Theo dõi vị trí chuột và xác định khi nào chuột vượt qua ranh giới màn hình
   - Quản lý việc chuyển đổi giữa điều khiển PC và điện thoại
   - Lưu trữ và áp dụng cấu hình ranh giới do người dùng thiết lập

3. **Network Manager**
   - Quản lý Socket Server để lắng nghe kết nối từ client Android
   - Xử lý việc gửi dữ liệu sự kiện đến client
   - Đảm bảo tính bảo mật và hiệu suất của kết nối

4. **UI Manager**
   - Cung cấp giao diện người dùng để cấu hình kết nối và ranh giới màn hình
   - Hiển thị trạng thái kết nối và thông tin debug
   - Quản lý các tùy chọn và cài đặt của ứng dụng

### Phía Android (Client)

1. **Network Client**
   - Kết nối với Socket Server trên PC
   - Nhận và giải mã dữ liệu sự kiện từ server
   - Quản lý việc kết nối lại khi mất kết nối

2. **Event Processor**
   - Xử lý các sự kiện chuột và bàn phím nhận được từ server
   - Chuyển đổi tọa độ chuột từ không gian PC sang không gian Android
   - Quyết định loại thao tác cần thực hiện trên Android

3. **Accessibility Service**
   - Mô phỏng các thao tác chạm, vuốt, và nhập liệu trên Android
   - Tương tác với hệ thống Android để thực hiện các thao tác
   - Theo dõi trạng thái UI để điều chỉnh các thao tác phù hợp

4. **Cursor Overlay**
   - Hiển thị con trỏ ảo trên màn hình Android
   - Di chuyển con trỏ theo dữ liệu vị trí chuột từ PC
   - Hiển thị trạng thái nút chuột (nhấn, thả)

## Luồng dữ liệu

1. **Bắt sự kiện nhập liệu (PC)**
   - Hook Manager bắt sự kiện chuột/bàn phím từ hệ thống Windows
   - Boundary Detector xác định xem sự kiện có nên được xử lý cục bộ hay gửi đến Android

2. **Truyền dữ liệu (PC → Android)**
   - Network Manager đóng gói sự kiện thành gói tin và gửi qua Socket
   - Network Client trên Android nhận và giải mã gói tin

3. **Xử lý sự kiện (Android)**
   - Event Processor phân tích sự kiện và quyết định hành động phù hợp
   - Cursor Overlay cập nhật vị trí con trỏ ảo
   - Accessibility Service thực hiện các thao tác tương ứng trên hệ thống Android

## Giao thức truyền thông

Giao thức truyền thông giữa PC và Android sử dụng Socket TCP/IP với định dạng gói tin JSON:

```json
{
  "type": "mouse_move|mouse_click|key_press|key_release",
  "timestamp": 1234567890,
  "data": {
    // Dữ liệu cụ thể cho từng loại sự kiện
    // Ví dụ cho mouse_move:
    "x": 100,
    "y": 200,
    // Ví dụ cho mouse_click:
    "button": "left|right|middle",
    "state": "down|up",
    // Ví dụ cho key_press/key_release:
    "key_code": 65,
    "modifiers": ["shift", "ctrl", "alt"]
  }
}
```

## Các mẫu thiết kế

1. **Observer Pattern**
   - Sử dụng trong Hook Manager để theo dõi và phản ứng với các sự kiện hệ thống
   - Cho phép các thành phần khác đăng ký nhận thông báo khi có sự kiện xảy ra

2. **Command Pattern**
   - Sử dụng trong Event Processor để chuyển đổi sự kiện thành các lệnh cụ thể
   - Cho phép đóng gói các thao tác thành các đối tượng, giúp dễ dàng mở rộng và bảo trì

3. **Singleton Pattern**
   - Áp dụng cho các manager chính như Hook Manager, Network Manager
   - Đảm bảo chỉ có một instance duy nhất của các thành phần quan trọng

4. **Factory Pattern**
   - Sử dụng để tạo các đối tượng sự kiện khác nhau dựa trên dữ liệu nhận được
   - Giúp tách biệt logic tạo đối tượng khỏi logic xử lý

5. **Strategy Pattern**
   - Áp dụng cho việc mô phỏng các thao tác khác nhau trên Android
   - Cho phép thay đổi thuật toán mô phỏng dựa trên ngữ cảnh và loại sự kiện

## Quyết định kỹ thuật chính

1. **Sử dụng Windows Hook API thay vì DirectInput**
   - Lý do: Hook API cho phép bắt sự kiện toàn hệ thống, không chỉ giới hạn trong ứng dụng
   - Ưu điểm: Có thể bắt tất cả các sự kiện, kể cả khi ứng dụng không focus
   - Nhược điểm: Có thể ảnh hưởng đến hiệu suất hệ thống nếu không được tối ưu hóa

2. **Sử dụng Accessibility Service trên Android**
   - Lý do: Đây là cách duy nhất để mô phỏng các thao tác nhập liệu mà không cần root
   - Ưu điểm: Hoạt động trên hầu hết các thiết bị Android không cần quyền đặc biệt
   - Nhược điểm: Có một số hạn chế về loại thao tác có thể thực hiện

3. **Sử dụng Socket TCP thay vì UDP**
   - Lý do: Đảm bảo độ tin cậy của dữ liệu, không bị mất gói tin
   - Ưu điểm: Đảm bảo tất cả các sự kiện đều được xử lý theo đúng thứ tự
   - Nhược điểm: Có thể có độ trễ cao hơn so với UDP

4. **Hiển thị con trỏ ảo trên Android**
   - Lý do: Cung cấp phản hồi trực quan cho người dùng về vị trí chuột
   - Ưu điểm: Trải nghiệm người dùng tốt hơn, dễ dàng định vị chuột
   - Nhược điểm: Tăng độ phức tạp của ứng dụng và có thể gây xung đột với một số ứng dụng khác

## Các ràng buộc và thách thức

1. **Độ trễ**
   - Thách thức: Giảm thiểu độ trễ giữa thao tác trên PC và phản hồi trên Android
   - Giải pháp: Tối ưu hóa giao thức truyền thông, sử dụng buffer và dự đoán chuyển động

2. **Tương thích với các phiên bản Android**
   - Thách thức: Đảm bảo ứng dụng hoạt động trên nhiều phiên bản Android khác nhau
   - Giải pháp: Sử dụng các API cấp thấp và kiểm tra tương thích trên nhiều thiết bị

3. **Bảo mật**
   - Thách thức: Đảm bảo kết nối an toàn và ngăn chặn các bên thứ ba can thiệp
   - Giải pháp: Mã hóa dữ liệu, xác thực kết nối, và giới hạn phạm vi kết nối

4. **Hiệu suất**
   - Thách thức: Giảm thiểu tác động đến hiệu suất hệ thống khi bắt và xử lý sự kiện
   - Giải pháp: Tối ưu hóa code, sử dụng threading và các kỹ thuật non-blocking