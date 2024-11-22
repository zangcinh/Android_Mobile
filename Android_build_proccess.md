# Mục lục

1. [Compiling Apps](#compiling-apps)
2. [APK Structure](#apk-structure)
3. [Code Signing](#code-signing)

# Compiling Apps

- Quá trình chuyển đổi thành source code thành 1 file cài đặt mà thiết bị Android có thể thực thi được. Quá trình đó gồm 2 bước chính là biên dịch và đóng gói, sau đó là ký ứng dụng 
  
![image](https://github.com/user-attachments/assets/55ee17eb-4865-428b-807f-e53aef6c2001)

  - Đầu tiên, Android Asset Packaging Tool (aapt) sẽ đóng gói các tệp tài nguyên và tạo ra tệp R.java
  - Nếu dự án sử dụng dịch vụ do AIDL (Android Interface Definition Language) cung cấp, cần sử dụng công cụ AIDL để phân tích tệp giao diện AIDL và tạo mã Java tương ứng
```
AIDL là 1 phần mềm được sử dụng để thiết lập giao tiếp giữa các ứng dụng khác nhau
```

  - Biên dịch các tệp R.java và AIDL thành tệp `.class` bằng Java Compiler
  - Sử dụng công cụ `dx` để chuyển tệp `.class` và các thư viện bên thứ 3 thành tệp `dex`
  - Sử dụng apkbuilder để đóng gói tài nguyên được biên dịch, tệp `.dex` và 1 số tài nguyên khác để tạo thành tệp APK

# APK Structure

- Sau khi ứng dụng Android được biên dịch sẽ tạo thành file APK (Android Package). File bao gồm các tài nguyên cần thiết để chạy chương trình như code và tài nguyên( ví dụ như hình ảnh )
- Để xem được nội dung trong file APK, đầu tiên bạn cần đổi đuôi file thành `.zip` và giải nén.
- Các nội dung trong file APK
  - AndroidManifest.xml: tệp này mô tả tên, phiên bản, quyền truy cập, thư viện và các nội dung khác của tệp APK
  - assets/: thư mục này chứa các tệp tài nguyên của ứng dụng, được bao gồm trong ứng dụng
  - classes.dex: đây là lớp Java đã được biên dịch thành định djang DEX (Dalvik Excutable)
  - lib/: thư mục này chứa mã đã biên dịch phụ thuộc vào nền tảng và các thư viện gốc dành cho kiến trúc thiết bị cụ thể, ví dụ: x86, x86_64
  - META-INF/: thư mục này chứa chứng chỉ ứng dụng, tệp manifest, chữ kí và danh sách các tài nguyên
  - res/: đây là thư mục chứa các tài nguyên, vdu như hình ảnh, những tài nguyên chưa được biên dịch vào trong `resources.arsc`
  - resources.arsc: chứa các tài nguyên đã được biên dịch trước mà ứng dụng sử dụng, ví dụ: chuỗi văn bản, màu sắc, kích thước,...

 **1. AndroidManifest.xml**
- Đây là file định nghĩa, kê khai mô tả thông tin thiết yếu về ứng dụng
- Ví dụ
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-feature android:name="android.hardware.camera" android:required="false" />
    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="StringEE"
        android:supportsRtl="true"
        android:theme="@style/Theme.StringEE"
        tools:targetApi="31">
        <activity
            android:name=".MainActivity"
            android:exported="true"
            tools:ignore="MissingClass">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```
a) Thẻ manifest

- Đây là phần tử đầu tiên trong tệp tin AndroidManifest.xml, chỉ định vị trí cho các thuộc tính liên quan đến Android trong tệp
- `xmlns:android`, `xmls:tools`: dòng khai báo namespace Android. Sau khi khai báo thì các thẻ bên trong manifest sẽ sử dụng được từ khóa android

b) Thẻ uses-permission 

- Những quyền mà hệ thống hiển thị ra đều lấy từ thẻ này. Ví dụ, ứng dụng Messenger khi sử ụng máy ảnh, ứng dụng sẽ hỏi người dùng với 3 lựa chọn: Được quyền khi sử dụng app - Chỉ lần này - Không được phép

c) Thẻ permission

-  Thẻ này dùng để khai báo ứng dụng cần một số quyền cụ thể để truy cập các tính năng hoặc dữ liệu nhạy cảm trên thiết bị 
d) Thẻ application

- Thẻ này sẽ hiển thị các cấu hình của ứng dụng
  - `Android:allowBackup`: nếu là `true` cho phép ứng dụng sao lưu dữ liệu khi người dùng sử dụng tính năng sao lưu (backup) của hệ điều hành Android. Nếu là `false`, dữ liệu không được sao lưu
  - `android:dataExtractionRules="@xml/data_extraction_rules"`: trỏ đến tệp XML định nghĩa các quy tắc trích xuất dữ liệu
  - `android:fullBackupContent="@xml/backup_rules"`: trỏ đến tệp XML mô tả chi tiết những dữ liệu nào trong ứng dụng được và không được sao lưu
  - `android:icon="@mipmap/ic_launcher"`: biểu tượng icon của ứng dụng ( biểu tượng hiển thị trên màn hình chính của thiết bị )
e) Thẻ Activity

- Thẻ định nghĩa 1 hoạt động trong ứng dụng

**2. Classes.dex**

- 
