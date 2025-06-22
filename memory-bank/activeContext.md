# Active Context: Share Mouse with Phone

## Trọng tâm công việc hiện tại

Dự án "Share Mouse with Phone" đang ở giai đoạn khởi đầu, với trọng tâm hiện tại là:

1. **Thiết lập cơ sở dự án**
   - Tạo repository và cấu trúc dự án cơ bản
   - Xác định kiến trúc và các thành phần chính
   - Lập tài liệu yêu cầu và thiết kế

2. **Phát triển Proof of Concept**
   - Xây dựng ứng dụng Windows đơn giản để bắt sự kiện chuột/bàn phím
   - Xây dựng ứng dụng Android đơn giản để hiển thị con trỏ ảo
   - Thiết lập kết nối Socket cơ bản giữa hai ứng dụng

## Những thay đổi gần đây

1. **Tạo tài liệu dự án**
   - Tạo README.md với mô tả tổng quan về dự án
   - Thiết lập ngân hàng bộ nhớ với các tài liệu cơ bản

2. **Nghiên cứu kỹ thuật**
   - Xác định các API cần thiết cho Windows Hook
   - Xác định cách sử dụng Accessibility Service trên Android
   - Đánh giá các phương pháp giao tiếp mạng

## Các bước tiếp theo

1. **Phát triển ứng dụng Windows (Server)**
   - Tạo dự án C# WPF cơ bản
   - Triển khai hệ thống hook chuột và bàn phím
   - Xây dựng logic phát hiện ranh giới màn hình
   - Thiết lập Socket Server

2. **Phát triển ứng dụng Android (Client)**
   - Tạo dự án Android cơ bản
   - Triển khai Accessibility Service
   - Xây dựng hệ thống hiển thị con trỏ ảo
   - Thiết lập Socket Client

3. **Tích hợp và thử nghiệm**
   - Kết nối hai ứng dụng và thử nghiệm truyền dữ liệu
   - Thử nghiệm các thao tác chuột và bàn phím cơ bản
   - Đánh giá độ trễ và hiệu suất

## Các quyết định và cân nhắc đang hoạt động

1. **Phương pháp mô phỏng thao tác trên Android**
   - **Quyết định hiện tại**: Sử dụng Accessibility Service để mô phỏng các thao tác chạm và nhập liệu
   - **Cân nhắc**: Accessibility Service có một số hạn chế, nhưng là cách duy nhất để mô phỏng thao tác mà không cần root
   - **Thử nghiệm cần thiết**: Đánh giá khả năng và giới hạn của Accessibility Service với các loại thao tác khác nhau

2. **Giao thức truyền thông**
   - **Quyết định hiện tại**: Sử dụng Socket TCP/IP với định dạng dữ liệu JSON
   - **Cân nhắc**: TCP đảm bảo độ tin cậy nhưng có thể có độ trễ cao hơn UDP
   - **Thử nghiệm cần thiết**: So sánh hiệu suất giữa TCP và UDP, đánh giá tác động của định dạng dữ liệu đến độ trễ

3. **Phương pháp phát hiện ranh giới màn hình**
   - **Quyết định hiện tại**: Sử dụng tọa độ X cố định do người dùng cấu hình
   - **Cân nhắc**: Cần hỗ trợ cấu hình linh hoạt cho nhiều màn hình và vị trí tương đối khác nhau
   - **Thử nghiệm cần thiết**: Thử nghiệm với các cấu hình màn hình khác nhau

4. **Hiển thị con trỏ ảo trên Android**
   - **Quyết định hiện tại**: Sử dụng WindowManager để hiển thị overlay
   - **Cân nhắc**: Overlay có thể gây xung đột với một số ứng dụng
   - **Thử nghiệm cần thiết**: Đánh giá tính tương thích với các ứng dụng phổ biến

## Các mẫu và sở thích quan trọng

1. **Mẫu thiết kế code**
   - Sử dụng MVVM (Model-View-ViewModel) cho ứng dụng Windows
   - Sử dụng MVVM hoặc MVC cho ứng dụng Android
   - Ưu tiên các nguyên tắc SOLID và clean code

2. **Quy ước đặt tên**
   - Tuân thủ quy ước đặt tên chuẩn cho C# và Kotlin
   - Sử dụng PascalCase cho C# và camelCase cho Kotlin
   - Đặt tên rõ ràng và mô tả chức năng

3. **Quản lý mã nguồn**
   - Sử dụng Git với các nhánh feature, develop, và main
   - Commit thường xuyên với mô tả rõ ràng
   - Code review trước khi merge

4. **Logging và Debug**
   - Sử dụng NLog cho ứng dụng Windows
   - Sử dụng Timber cho ứng dụng Android
   - Log đầy đủ thông tin để dễ dàng debug

## Bài học kinh nghiệm và hiểu biết sâu sắc

1. **Thách thức kỹ thuật**
   - Mô phỏng thao tác chuột trên Android là một thách thức lớn do giới hạn của hệ điều hành
   - Độ trễ mạng có thể ảnh hưởng đáng kể đến trải nghiệm người dùng
   - Cần cân bằng giữa tính năng và hiệu suất

2. **Cơ hội cải tiến**
   - Có thể mở rộng để hỗ trợ chia sẻ clipboard
   - Có thể thêm tính năng chia sẻ file
   - Có thể phát triển phiên bản cho các hệ điều hành khác (macOS, Linux)

3. **Rủi ro đã xác định**
   - Các phiên bản Android mới có thể thay đổi cách Accessibility Service hoạt động
   - Một số ứng dụng có thể chặn Accessibility Service
   - Kết nối mạng không ổn định có thể gây ra trải nghiệm không tốt

## Trạng thái hiện tại của dự án

1. **Hoàn thành**
   - Tài liệu yêu cầu và thiết kế cơ bản
   - Nghiên cứu kỹ thuật ban đầu

2. **Đang tiến hành**
   - Thiết lập cấu trúc dự án
   - Phát triển Proof of Concept

3. **Chưa bắt đầu**
   - Phát triển đầy đủ các ứng dụng
   - Thử nghiệm và tối ưu hóa
   - Phát hành

## Các vấn đề cần giải quyết

1. **Kỹ thuật**
   - Làm thế nào để giảm thiểu độ trễ trong truyền dữ liệu?
   - Làm thế nào để mô phỏng chính xác các thao tác chuột phức tạp trên Android?
   - Làm thế nào để đảm bảo kết nối an toàn giữa PC và điện thoại?

2. **Trải nghiệm người dùng**
   - Làm thế nào để cấu hình ranh giới màn hình một cách trực quan?
   - Làm thế nào để chuyển đổi liền mạch giữa PC và điện thoại?
   - Làm thế nào để hiển thị con trỏ ảo mà không gây xao nhãng?

3. **Triển khai**
   - Làm thế nào để đảm bảo ứng dụng hoạt động trên nhiều phiên bản Windows và Android?
   - Làm thế nào để đơn giản hóa quá trình cài đặt và cấu hình?
   - Làm thế nào để phân phối ứng dụng một cách hiệu quả?