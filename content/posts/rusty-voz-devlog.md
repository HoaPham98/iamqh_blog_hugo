---
title: "#2: Flutter Rusty Voz - Hacking the Mechanics & Native Experience 🛠️🦀"
date: 2026-01-01 16:10:34
tags: ["devlog", "rusty-voz", "rust", "flutter"]
series: ["rusty-voz-devlog"]
aliases:
  - /rust-voz/
---

Chào anh em, hôm nay mình sẽ đào sâu hơn vào **kỹ thuật** đằng sau *Flutter Rusty Voz*. Không lan man về Flutter cơ bản, bài viết này sẽ xoáy sâu vào cách mình "reverse engineering" cơ chế của Voz để mang lại trải nghiệm Native tốt nhất.

## 1. Deep Dive: Voz Mechanics & Authentication 🔐

Voz sử dụng xenForo làm nền tảng, và việc giao tiếp API với nó không đơn thuần là RESTful chuẩn chỉ. Đó là cuộc chơi của Cookies, Tokens và... HTML Parsing.

### Login & Session Management
Voz không cung cấp Public API cho 3rd-party. Để đăng nhập, app phải giả lập hành vi của một trình duyệt:
1.  **Form Submission**: Gửi request POST tới endpoint login của xenForo với `login`, `password`.
2.  **Cookie Handling**: Hệ thống sử dụng cơ chế **Cookie Jar** tự động. Mọi cookie trả về từ server (như `xf_user`, `xf_session`) đều được lưu trữ an toàn và tự động đính kèm vào header của các request tiếp theo, đảm bảo tính liên tục của phiên đăng nhập.
3.  **Automatic CSRF Extraction**: Đây là "vũ khí bí mật". Mình cài đặt một cơ chế **HTTP Middleware** ở tầng network. Mỗi khi nhận response HTML, nó sẽ quét và trích xuất token `data-csrf` ẩn trong thẻ `<html>`. Token này sau đó được tự động inject vào header của mọi request POST. Nhờ vậy, session luôn hợp lệ mà không cần code xử lý thủ công ở từng API call.

### Thử thách MFA (Multi-Factor Authentication) 🛡️
Cơ chế bảo mật 2 lớp của Voz khá thú vị. Khi login, nếu server phát hiện account bật 2FA, nó không trả về lỗi 401 mà trả về một trang HTML yêu cầu nhập code (hoặc redirect).
-   **Client Side**: App sẽ phân tích URL hoặc nội dung HTML trả về để phát hiện trạng thái này. Nếu phát hiện yêu cầu 2FA, app sẽ tạm dừng flow login và chuyển sang trạng thái chờ nhập code.
-   **MFA Flow**: Khi người dùng nhập code, app gửi request tới endpoint xác thực riêng biệt. Chỉ khi bước này thành công, server mới cấp các cookie xác thực cuối cùng.

## 2. Xây dựng Post Content Native (No WebView) 📱

Tại sao nhiều app diễn đàn dùng `WebView`? Vì nó dễ. Render HTML là việc của browser. Nhưng `WebView` nặng, scroll không mượt đồng bộ với list native, và khó customize gesture.

*Flutter Rusty Voz* chọn con đường khó hơn: **Native Rendering**.

### Giải pháp: DOM Tree to Widget Tree
Thay vì dùng WebView, mình xây dựng một bộ parser để chuyển đổi trực tiếp cấu trúc **DOM HTML** thành **Widget Tree** của Flutter:
-   **Recursive Mapping**: Duyệt cây DOM và map từng thẻ HTML (`<div>`, `<p>`, `<blockquote>`) thành các layout native tương ứng như `Column`, `RichText`, `Container`.
-   **Smart Caching**: Các thẻ `<img>` được thay thế bằng các widget hiển thị ảnh có tích hợp sẵn layer caching vào bộ nhớ và ổ đĩa, giúp load lại nội dung tức thì khi cuộn trang.
-   **Custom Interaction**: Các thẻ `<a>` được chặn (intercept) hành vi mặc định. Nếu là link nội bộ vOz, app sẽ thực hiện navigation native trong app thay vì mở trình duyệt.

### Tối ưu hóa trải nghiệm
-   **Quote & Spoilers**: Các block này được style lại hoàn toàn bằng native widget, giúp hiển thị gọn gàng và có thể mở rộng/thu gọn mượt mà.
-   **Code Blocks**: App tự động phát hiện các đoạn code, phân tích ngôn ngữ và áp dụng **Syntax Highlighting** (tô màu cú pháp) ngay trên native view.
-   **Image Viewer**: Tích hợp trình xem ảnh native với khả năng zoom/pan mượt mà, hỗ trợ vuốt để đóng, tốt hơn hẳn trải nghiệm click vào ảnh trên web mobile.

## 3. Screenshots 📸

Dưới đây là một số màn hình demo của app đang chạy thực tế:

| Forum List View | Native Thread Rendering - Post 1 | Native Thread Rendering - Post 2 | Native Thread Rendering - Post 3 |
| --------------- | -------------------------------- | -------------------------------- | -------------------------------- |
| ![Forum List](/rusty-voz/screenshot/screenshot_forum.jpg) | ![Thread View 1](/rusty-voz/screenshot/screenshot_native_thread.jpg) | ![Thread View 2](/rusty-voz/screenshot/screenshot_native_thread_2.jpg) | ![Thread View 3](/rusty-voz/screenshot/screenshot_native_thread_3.jpg) |

## 4. TODO List 📝

Dự án vẫn đang tiếp tục phát triển. Đây là lộ trình sắp tới:

- [ ] **Notification System**: Push notification khi có quote hoặc reply (Cực khó vì cần service background).
- [ ] **Advanced Posting**: Upload ảnh trực tiếp lên 2.pik.vn hoặc Imgur từ app.
- [ ] **Offline Mode**: Lưu thread để đọc khi mất mạng.
- [ ] **Tablet Support**: Layout 2 cột cho iPad/Tablet.
- [ ] **Account Manager**: Chuyển đổi nhanh giữa nhiều tài khoản (multi-account).

## 5. Downloads ⬇️

Anh em có thể tải bản build mới nhất tại đây (Hỗ trợ Android & iOS):

-   **Android (APK)**: https://github.com/iampqh/rusty-voz/releases

---
*Built with ❤️ using Flutter & Rust.*
