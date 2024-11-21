# MỤC LỤC

1. [Tổng quan](#tổng-quan)
2. [Android Architecture](#android-architecture)
3. [Android Virtual Machine](#android-virtual-machine)
4. [Android Security Model](#android-security-model)


# 1. Tổng quan

- Các thành phần chính trong hệ điều hành Android 

![image](https://github.com/user-attachments/assets/2a49f6d9-6bd0-4c92-8141-f889c79ba485)

a) Linux kernel 

- linux kernel chính là nền tảng cho hệ điều hành Android. Chúng là chương trìn lõi để quản lý tài nguyên CPU, bộ nhớ hệ thống, thiết bị hệ thống bao gồm các hệ thống file và kết nối mạng, quản lý các tiến trình. 
- Khi khởi động 1 ứng dụng, kernel sẽ tải ứng dụng đó vào trong bộ nhớ, tạo ra các tiến trình cần thiết và khởi động để ứng dụng chạy. Khi ứng dụng cần bộ nhớ, kernel sẽ phân bổ cho nó. Khi ứng dụng cần kết nối mạng, kernel sẽ làm tất cả các tác vụ xử lý cấp

```
Tác vụ xử lý cấp thấp là những hoạt động xử lý trực tiếp liên quan đến phần cứng. Ví dụ: quản lý bộ nhớ, quản lý phần cứng, xử lý hệ thống tập tin, điều khiển CPU, xử lý mạng,...
```
- Các thành phần chủ yếu
  - Display driver: điều khiển việc hiển thị màn hình, thu nhận những điều khiển của người dùng trên màn hình ( di chuyển, cảm ứng,..)
  - Camera Driver: Điều khiển hoạt động của camera, nhận luồng dữ liệu camera trả về
  - Bluetooth Driver: Điều khiển thiết bị thu, phát song camera
  - USB Driver: Quản lý hoạt động của các cổng giao tiếp USB
  - Keypad Driver: Điều khiển bàn phím
  - Wifi Driver: thu phát sóng 
b) Hardware Abstraction Layer (HAL)

- HAL là 1 lớp phần mềm trong Android giúp trừu tượng hóa phần cứng, cho phép các thành phần phần mềm cấp cao ( ứng dụng, dịch vụ hệ thống,... ) giao tiếp với phần cứng một cách dễ dàng và không cần biết chi tiết cụ thể về phần cứng
- Chức năng: cung cấp giao diện nhất quán để các dịch vụ hệ thống Android có thể tương tác với phần cứng
- Ví dụ hoạt động
```
Ứng dụng Camera -> Frameword API ->  HAL -> Driver phần cứng -> Cảm biến camera
```
Ứng dụng chỉ cần gọi các phương thức từ API mà không cần biết cảm biến là loại gì hay các giao tiếp với nhau

` Driver là phần mềm cho phép hệ điều hành và các chương trình khác trên máy tính có thể điều khiển được phần cứng và giúp phần cứng + hệ điều hành giao tiếp với nhau`

`API là phương thức trung gian kết nối các ứng dụng, thư viện khác nhau, cung cấp khả năng truy xuất đén 1 tập các hàm hay dùng, từ đó có thể trao đổi dữ liệu giữa các ứng dụng`

c) Android Runtime (ART)
- ART là môi trường thực thi ứng dụng được sử dụng trên hệ điều hành Android giúp tăng tốc và tối ưu hóa việc chạy ứng dụng trên thiết bị
- Chức năng: thực thi ứng dụng, quản lý bộ nhớ, biên dịch mã

d) Native C/C++ libraries
- Phần này có nhiều thư viện được viết bằng C/C++ để các phần mềm có thể sử dụng, các thư viện đó được tập hợp thành một số nhóm như
  - Thư viện hệ thống (System C library): được sử dụng bởi hệ điều hành
  - Thư viện media ( Media Libraries ): Có nhiều code C để hỗ trợ việc phát và ghi các định dạng âm thanh, hình ảnh, video thông dụng
  - Thư viện web (LibWebCore): đây là thành phần để xem nội dung trên web, được sử dụng để xây dựng phần mềm duyệt web (Android Browse. Nó hỗ trợ được nhiều công nghệ như HTML5, JS,CSS DOM, AJAX,..
  - Thư viện SQLite: hệ cơ sở dữ liệu để các ứng dụng có thể sử dụng

 e) Java API framework 
 - Các tính năng của Android được thông qua API được viết bằng ngôn ngữ Java. API là thành phần quan trọng để tạo nên ứng dụng Android, bao gồm
  - Xây dựng giao diện người dùng (UI) của ứng dụng, bao gồm danh sách, hộp văn bản, nút bấm,..
  - Trình quản lý tài nguyên (resource manager), cung cấp quyền truy cập vào các tài nguyên không phải mã, như văn bản, hình ảnh,...
  - Trình quản lý thông báo (notification manager) cho phép tất cả ứng dụng hiển thị thông báo trên thanh trạng thái
  - Trình quản lý hoạt động (activity manager) quản lý vòng đời của ứng dụng
  - Các trình cung cấp nội dung (content providers) cho phép ứng dụng truy cập dữ liệu từ các ứng dụng khác, ví dụ: danh bạ

f) System apps
- Đây là phần ứng dụng giao tiếp trực tiếp với người sử dụng, bao gồm các ứng dụng như
  - Các ứng dụng cơ bản gắn liền với hệ điều hành: gọi điện, quản lý danh bạ, duyệt web, nhắn tin,...
  - Các ứng dụng được cài thêm như trò chơi, phần mềm học,..

# 2. Android Architecture
