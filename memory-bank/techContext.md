# Tech Context: Share Mouse with Phone

## Công nghệ sử dụng

### Phía Windows (Server)

1. **Ngôn ngữ và Framework**
   - **C#**: Ngôn ngữ lập trình chính cho ứng dụng Windows
   - **.NET Framework 4.7.2+**: Framework cơ bản cho ứng dụng
   - **WPF (Windows Presentation Foundation)**: Để xây dựng giao diện người dùng

2. **API và Thư viện**
   - **Windows API (User32.dll)**: Để bắt sự kiện chuột và bàn phím toàn hệ thống
   - **SetWindowsHookEx**: API chính để thiết lập hook chuột và bàn phím
   - **System.Net.Sockets**: Để xây dựng kết nối Socket TCP/IP
   - **Newtonsoft.Json**: Để xử lý dữ liệu JSON

3. **Công cụ phát triển**
   - **Visual Studio**: IDE chính cho phát triển
   - **Git**: Quản lý mã nguồn
   - **NuGet**: Quản lý gói và thư viện

### Phía Android (Client)

1. **Ngôn ngữ và Framework**
   - **Kotlin**: Ngôn ngữ lập trình chính cho ứng dụng Android
   - **Android SDK**: Phát triển ứng dụng Android
   - **Android Jetpack**: Bộ thư viện và công cụ hiện đại cho Android

2. **API và Thư viện**
   - **Accessibility Service API**: Để mô phỏng các thao tác chạm và nhập liệu
   - **WindowManager**: Để hiển thị overlay (con trỏ ảo)
   - **Socket API**: Để kết nối với server trên PC
   - **Gson/Moshi**: Để xử lý dữ liệu JSON
   - **Coroutines**: Để xử lý bất đồng bộ và threading

3. **Công cụ phát triển**
   - **Android Studio**: IDE chính cho phát triển
   - **Gradle**: Hệ thống build
   - **Git**: Quản lý mã nguồn

## Thiết lập phát triển

### Yêu cầu phát triển Windows

1. **Phần cứng**
   - Máy tính chạy Windows 10 hoặc cao hơn
   - RAM tối thiểu 8GB
   - Không gian đĩa trống tối thiểu 10GB

2. **Phần mềm**
   - Visual Studio 2019 hoặc cao hơn
   - .NET Framework 4.7.2 SDK
   - Git

3. **Cấu hình**
   - Cài đặt các workload "Desktop development with C#" trong Visual Studio
   - Cài đặt các gói NuGet cần thiết

### Yêu cầu phát triển Android

1. **Phần cứng**
   - Máy tính với RAM tối thiểu 8GB
   - Không gian đĩa trống tối thiểu 10GB
   - Thiết bị Android thật hoặc máy ảo để thử nghiệm

2. **Phần mềm**
   - Android Studio Chipmunk (2021.2.1) hoặc cao hơn
   - Android SDK Platform 26 (Android 8.0) hoặc cao hơn
   - JDK 11
   - Git

3. **Cấu hình**
   - Cài đặt Android SDK Platform 26+
   - Cài đặt Android SDK Build-Tools
   - Cài đặt Android Emulator (nếu không có thiết bị thật)

## Ràng buộc kỹ thuật

### Phía Windows

1. **Quyền hệ thống**
   - Ứng dụng cần quyền Administrator để thiết lập global hooks
   - Cần được phép bởi tường lửa để mở cổng TCP

2. **Hiệu suất**
   - Hook chuột/bàn phím phải có độ trễ thấp để đảm bảo trải nghiệm người dùng
   - Xử lý sự kiện phải hiệu quả để không ảnh hưởng đến hiệu suất hệ thống

3. **Tương thích**
   - Phải hoạt động trên Windows 10 và 11
   - Phải xử lý đúng các cấu hình nhiều màn hình

### Phía Android

1. **Quyền hệ thống**
   - Cần quyền BIND_ACCESSIBILITY_SERVICE để mô phỏng thao tác nhập liệu
   - Cần quyền SYSTEM_ALERT_WINDOW để hiển thị overlay
   - Cần quyền INTERNET để kết nối mạng

2. **Giới hạn API**
   - Accessibility Service có giới hạn về loại thao tác có thể thực hiện
   - Không thể mô phỏng hoàn hảo tất cả các thao tác chuột trên Android

3. **Tương thích**
   - Phải hoạt động trên Android 8.0 (API 26) trở lên
   - Phải xử lý đúng các kích thước màn hình và mật độ điểm ảnh khác nhau

## Phụ thuộc

### Phía Windows

```xml
<!-- packages.config -->
<packages>
  <package id="Newtonsoft.Json" version="13.0.1" targetFramework="net472" />
  <package id="NLog" version="4.7.15" targetFramework="net472" />
  <package id="System.Reactive" version="5.0.0" targetFramework="net472" />
</packages>
```

### Phía Android

```gradle
// build.gradle
dependencies {
    implementation 'androidx.core:core-ktx:1.8.0'
    implementation 'androidx.appcompat:appcompat:1.5.0'
    implementation 'com.google.android.material:material:1.6.1'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.6.4'
    implementation 'com.squareup.moshi:moshi-kotlin:1.13.0'
}
```

## Mẫu sử dụng công cụ

### Windows Hook API

```csharp
// Thiết lập low-level mouse hook
private static IntPtr SetMouseHook(LowLevelMouseProc proc)
{
    using (Process curProcess = Process.GetCurrentProcess())
    using (ProcessModule curModule = curProcess.MainModule)
    {
        return SetWindowsHookEx(WH_MOUSE_LL, proc,
            GetModuleHandle(curModule.ModuleName), 0);
    }
}

// Callback xử lý sự kiện chuột
private static IntPtr MouseHookCallback(int nCode, IntPtr wParam, IntPtr lParam)
{
    if (nCode >= 0)
    {
        MSLLHOOKSTRUCT hookStruct = (MSLLHOOKSTRUCT)Marshal.PtrToStructure(lParam, typeof(MSLLHOOKSTRUCT));
        // Xử lý sự kiện chuột
    }
    return CallNextHookEx(_mouseHookID, nCode, wParam, lParam);
}
```

### Android Accessibility Service

```kotlin
class MouseAccessibilityService : AccessibilityService() {
    override fun onAccessibilityEvent(event: AccessibilityEvent) {
        // Xử lý sự kiện accessibility
    }

    override fun onInterrupt() {
        // Xử lý khi service bị gián đoạn
    }

    // Mô phỏng thao tác chạm
    fun performTap(x: Float, y: Float) {
        val path = Path()
        path.moveTo(x, y)
        val gestureBuilder = GestureDescription.Builder()
        gestureBuilder.addStroke(GestureDescription.StrokeDescription(path, 0, 100))
        dispatchGesture(gestureBuilder.build(), null, null)
    }
}
```

## Các vấn đề kỹ thuật đã biết

1. **Độ trễ mạng**
   - Vấn đề: Kết nối TCP/IP có thể gây độ trễ, ảnh hưởng đến trải nghiệm người dùng
   - Giải pháp: Tối ưu hóa giao thức, sử dụng buffer và dự đoán chuyển động

2. **Giới hạn của Accessibility Service**
   - Vấn đề: Không thể mô phỏng tất cả các thao tác chuột phức tạp
   - Giải pháp: Tập trung vào các thao tác cơ bản và phổ biến nhất

3. **Tương thích với các ứng dụng Android**
   - Vấn đề: Một số ứng dụng có thể không phản hồi đúng với các sự kiện từ Accessibility Service
   - Giải pháp: Thử nghiệm với nhiều ứng dụng phổ biến và điều chỉnh cách mô phỏng sự kiện

4. **Bảo mật kết nối**
   - Vấn đề: Kết nối không được mã hóa có thể bị nghe lén
   - Giải pháp: Triển khai mã hóa TLS/SSL cho kết nối Socket

## Kế hoạch kỹ thuật

1. **Giai đoạn 1: Proof of Concept**
   - Xây dựng ứng dụng Windows cơ bản để bắt sự kiện chuột/bàn phím
   - Xây dựng ứng dụng Android cơ bản để hiển thị con trỏ ảo
   - Thiết lập kết nối Socket đơn giản giữa hai ứng dụng

2. **Giai đoạn 2: Phát triển cốt lõi**
   - Hoàn thiện hệ thống hook trên Windows
   - Phát triển Accessibility Service trên Android
   - Xây dựng giao thức truyền thông đầy đủ

3. **Giai đoạn 3: Tối ưu hóa và UI**
   - Tối ưu hóa hiệu suất và giảm độ trễ
   - Phát triển giao diện người dùng đầy đủ
   - Thêm các tùy chọn cấu hình

4. **Giai đoạn 4: Thử nghiệm và hoàn thiện**
   - Thử nghiệm trên nhiều thiết bị và phiên bản hệ điều hành
   - Sửa lỗi và cải thiện trải nghiệm người dùng
   - Chuẩn bị cho phát hành