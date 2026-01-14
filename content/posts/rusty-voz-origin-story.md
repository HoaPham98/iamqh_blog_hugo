---
title: "#1: Flutter Rusty Voz - Lý Do & Sự Ra Đời 🌱🦀"
date: 2025-12-25 00:00:00
tags: ["devlog", "rusty-voz", "rust", "flutter", "origin-story"]
series: ["rusty-voz-devlog"]
aliases:
  - /rust-voz/
---

Chào anh em, trước khi đi sâu vào kỹ thuật ở những bài viết sau, mình muốn kể chuyện về **lý do** tại sao lại có *Flutter Rusty Voz*. Đây là câu chuyện về một dự án bắt đầu từ curiosity, đi qua những moment "muốn bỏ cuộc", và cho đến nay... vẫn mãi mãi WIP.

## 1. Khởi Đầu - Tại Sao Làm Một App Voz? 🤔

Cuối 2023, mình đang dùng iPhone và có app NextVoz khá ổn. Ninja vào đọc xong rồi lặng lẽ đi ra =]]. Nhưng trong quá trình sử dụng, máu mobile developer nổi lên: *"Thành ra app này hoạt động như nào nhỉ?"*

Lúc đó app Android khá... lởm. MFA không được luôn. Mobile web của Voz thì cũng bình thường, không hẳn tệ (đặc biệt là trên máy tính, có ad blocker thì không có gì phàn nàn). Nhưng câu chuyện thực sự bắt đầu từ việc: **muốn luyện skill**.

Mình đang hay vào Voz hóng, muốn học thêm Rust mà không nghĩ ra ý tưởng gì. Và thế là...

## 2. Tại Sao Là Voz? 🎯

Tại sao không phải Tinh tế, TTVN, hay một forum nào khác?

Thật lòng mà nói, đơn giản là do mình hay đọc Voz. Và các Vozer thì **"trên thông thiên văn, dưới tường địa lý"** =]]. Đặc biệt là F33. Voz có cái vibe riêng mà forum khác không có.

Mình biết Voz từ hồi đại học, nhưng phải đến 2022 mới lập user. Vậy là cũng được vài năm rồi. Đủ để thấy cái forum này có gì đó đặc biệt, đáng để build một app cho nó.

## 3. Technology Choice - Tại Sao Lại Flutter + Rust? 🛠️

### Ban đầu là Rust + SwiftUI

Thú thật là ban đầu mình dự định làm một Voz client bằng Rust và tích hợp vào SwiftUI. Lúc đó cũng đang muốn học thêm SwiftUI, nên tính làm 2 món 1 lúc. Kill 2 birds with 1 stone.

### Nhưng rồi... chuyển sang Android

Sau khi bỏ iPhone chuyển sang Android (Vivo X200 Pro mini), mình phải rethink. Flutter là lựa chọn tự nhiên: **ổn định, crossplatform**. Một codebase, chạy được trên cả 2 platforms.

### Tại sao lại là Rust?

Ban đầu chỉ biết Rust qua tên =]]. Muốn học thêm ngôn ngữ mới, tận dụng được hiệu năng cao. Use case trong app này là một **client wrapper** đóng vai trò như network provider (giống dio vậy) cho Flutter app.

Mình có cân nhắc C++, Go... nhưng muốn tận dụng FFI để crossplatform. Vậy là Flutter + Rust it is.

### Challenge lớn nhất?

**Bridge giữ 2 language lại với nhau**. Flutter-Rust FFI setup thì không khó lắm, đã có lib support. Nhưng việc để 2 bên "nói chuyện" với nhau mượt mà... đó là chuyện khác.

## 4. Journey - Quá Trình Phát Triển 🚀

### MVP ban đầu

MVP ban đầu chỉ là một Rust client với các tính năng cơ bản: Login, Load thread, Load post.

GitHub repo (nếu ai muốn tham khảo): https://github.com/HoaPham98/voz-rust

### Tính năng đầu tiên: Login + MFA

**Login và MFA**. Mình nhớ như in lần đầu login + MFA thành công. Đó là milestone memorable nhất. Khi đó biết *"ổn rồi, this is gonna work"*.

### Thách thức lớn nhất: Parsing thread

**Parsing thread**. Mình đã bỏ tận 2 năm =]]. Cái moment đó thực sự khiến mình muốn bỏ.

Ngoài khó ra thì... một phần là do bận. Nhưng tại sao lại quay lại thời điểm này? Một phần là do đã có AI, muốn luyện vibe coding nữa, hehe.

### Anti-bot?

Có nếu dùng agent thông thường. Mình phải deepdive vào NextVoz để tìm cách bypass. Chắc tác giả bên đó đã liên hệ với admin rồi =]]

## 5. Tại Sao Native App? 💪

Tại sao không PWA? Tại sao không Hybrid app (WebView wrapper)?

**Tại thích challenge.** Mình quan tâm nhiều đến performance, muốn mang lại trải nghiệm mượt mà nhất có thể mà mobile web không làm được. Native app có performance tốt hơn, control hơn, và... challenge hơn.

## 6. Inspiration & Credit 🙏

NextVoz là inspiration lớn nhất. Mình muốn UI native nhất có thể.

Hiện tại đang có phần **emoji picker**, **reply editor** sử dụng WebView. Và phải thú thật là... **đang ĂN CẮP ý tưởng và code của bác HaoPhan** đang có app VozVn mà ae Vozer đang dùng rất nhiều.

> **Xin lỗi bác HaoPhan!** 🙏

## 7. Cảm Nghĩ Về Project 🎭

"Cũng vui vẻ."

Project này vẫn đang **WIP**. Có plan cho iOS release, more features... nhưng chưa open source (khả năng tương lai sẽ).

Đã học được nhiều: Flutter, Rust, FFI, scraping... và cả patience + persistence. Cũng đã có moment muốn bỏ, nhưng rồi lại tiếp tục. Đó cũng là một phần của journey.

## 8. What's Next? 🚧

Series devlog này sẽ tiếp tục với:

1. **Hacking the Mechanics & Native Experience** - Deep dive vào Voz API, authentication, và native rendering
2. **Multi-Account & Rust Power** - Kể chuyện về multi-account và tại sao Rust thắng so với WebView

Cảm ơn anh em đã đọc đến đây. Hẹn gặp lại ở những bài viết tiếp theo!

---

*Built with ❤️ using Flutter & Rust.*

## Downloads ⬇️

Anh em có thể tải bản build mới nhất tại đây:

- **Android (APK)**: https://github.com/iampqh/rusty-voz/releases
- **Project đang WIP**, hãy chờ nhiều tính năng mới!

---

*[P.S. Nếu bạn đang đọc bài này và muốn hỏi gì về project, feel free to reach out. Mình cũng vui vẻ được chia sẻ thêm!]*
