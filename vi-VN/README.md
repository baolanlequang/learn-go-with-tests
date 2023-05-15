# Learn Go with Tests

<p align="center">
  <img src="../red-green-blue-gophers-smaller.png" />
</p>

[Đồ hoạ thiết kế bởi Denise](https://twitter.com/deniseyu21)

[![Go Report Card](https://goreportcard.com/badge/github.com/quii/learn-go-with-tests)](https://goreportcard.com/report/github.com/quii/learn-go-with-tests)

## Các định dạng sách

- [Gitbook tiếng Anh](https://quii.gitbook.io/learn-go-with-tests)
- [EPUB hoặc PDF tiếng Anh](https://github.com/quii/learn-go-with-tests/releases)

## Bản dịch

- [Tiếng Trung](https://studygolang.gitbook.io/learn-go-with-tests)
- [Tiếng Bồ Đào Nha](https://larien.gitbook.io/aprenda-go-com-testes/)
- [Tiếng Nhật](https://andmorefine.gitbook.io/learn-go-with-tests/)
- [Tiếng Hàn](https://miryang.gitbook.io/learn-go-with-tests/)
- [Tiếng Thổ Nhĩ Kỳ](https://halilkocaoz.gitbook.io/go-programlama-dilini-ogren/)
- [Tiếng Việt](https://halilkocaoz.gitbook.io/go-programlama-dilini-ogren/)

## Hỗ trợ tôi

Tôi hân hạnh cung cấp miễn phí nguồn tài liệu này, nhưng nếu bạn có thể cảm ơn bằng các cách sau: 
I am proud to offer this resource for free, but if you wish to give some appreciation:

- [Theo dõi tôi trên Twitter @quii](https://twitter.com/quii)
- <a rel="me" href="https://mastodon.cloud/@quii">Mastodon</a>
- [Mua cho tôi một ly cà phê :coffee:](https://www.buymeacoffee.com/quii)
- [Tài trợ cho tôi trên GitHub](https://github.com/sponsors/quii)

## Tại sao

* Khám phá ngôn ngữ Go bằng cách viết test
* **Xây dựng nền tảng TDD**. Go là một ngôn ngữ tốt cho việc học TDD, bởi vì nó là một ngôn ngữ đơn giản và dễ học và các thư viện dùng để test được tích hợp sẵn.
* Tự tin rằng bạn sẽ có thể bắt đầu viết các hệ thống vững chắc, được test tốt bằng Go.
* [Xem video, hoặc đọc về tại sao viết unit test và TDD là quan trọng](why.md)

## Mục lục

### Cơ bản về Go

1. [Cài đặt Go](install-go.md) - Cài đặt môi trường cho sản phẩm.
2. [Hello, world](hello-world.md) - Khai báo các biến, hằng số, các câu lệnh if/else, switch, viết chương trình Go đầu tiên và viết test đầu tiên. Cú pháp sub-test và closure.
3. [Integers](integers.md) - Khám phá sâu hơn về cú pháp khai báo hàm và học thêm các cách mới để cải thiện việc tạo tài liệu cho code của bạn.
4. [Vòng lặp](iteration.md) - Học về vòng lặp `for` và thực hiện benchmark.
5. [Arrays and slices](arrays-and-slices.md) - Học về array, slice, `len`, varargs, `range` và test coverage.
6. [Structs, methods & interfaces](structs-methods-and-interfaces.md) - Học về `struct`, methods, `interface` và table driven tests.
7. [Pointers & errors](pointers-and-errors.md) - Học về pointers and errors.
8. [Maps](maps.md) - Học về cách lưu trữ dữ liệu trong cấu trúc dữ liệu map.
9. [Dependency Injection](dependency-injection.md) - Học về dependency injection, cách nó liên quan trong việc sử dụng các interface và một thành phần cơ bản trong io.
10. [Mocking](mocking.md) - Sử dụng các phần code đã có nhưng chưa được test  Take some existing untested code and use DI with mocking to test it.
11. [Concurrency](concurrency.md) - Học cách viết code concurrent để làm cho phần mềm của bạn chạy nhanh hơn.
12. [Select](select.md) - Học cách đồng bộ hoá (synchronise) các process asynchronous một cách xịn nhất.
13. [Reflection](reflection.md) - Cách dùng reflection
14. [Sync](sync.md) - Học cách dùng một vài tính năng từ package sync bao gồm `WaitGroup` và `Mutex`
15. [Context](context.md) - Sử dụng package context để kiểm soát và huỷ bỏ các process chạy trong thời gian dài.
16. [Giới thiệu property based tests](roman-numerals.md) - Thực hành một vài TDD với bài toán Số La Mã và hiểu sơ lược về property based tests
17. [Maths](math.md) - Sử dụng package `math` để vẽ một đồng hồ SVG
18. [Đọc file](reading-files.md) - Cách đọc các file và xử lý chúng
19. [Templating](html-templates.md) - Sử dụng package html/template của Go để tạo html từ dữ liệu, đồng thời tìm hiểu về approval testing
20. [Generics](generics.md) - Cách viết các hàm sử dụng các tham số generics và xây dựng cấu trúc dữ liệu generics cho riêng bạn
21. [Quay lại với array và slice cùng generics](revisiting-arrays-and-slices-with-generics.md) - Generics rất hữu dụng khi làm việc với các loại collection. Học cách viết hàm `Reduce` và áp dụng một vài pattern phổ biến.

### Xây dựng một ứng dụng

Bây giờ hy vọng rằng bạn đã rõ phần _Cơ bản về Go_ và bạn đã có một nền tảng về các tính năng chính của ngôn ngữ Go và cách dùng TDD.

Phần này sẽ nói về việc xây dựng một ứng dụng.

Mỗi chương sẽ dựa trên chương trước đó, và mở rộng các tính năng của ứng dụng mà chúng ta mong muốn.

Các khái niệm mới sẽ được giới thiệu để giúp viết code tiện hơn, nhưng hầu hết các tài liệu mới sẽ tìm hiểu về những gì có thể đạt được từ thư viện chuẩn của Go. 

Cuối phần này, sẽ nắm chắc các viết một ứng dụng bằng Go được bảo đảm bởi test.

* [HTTP server](http-server.md) - Chúng ta sẽ tạo một ứng dụng lắng nghe các HTTP request và respond các request đó.
* [JSON, routing và embedding](json.md) - Chúng ta sẽ tạo các endpoint trả về định dạng JSON và khám phá cách để thực hiện routing.
* [IO và sorting](io.md) - Chúng ta sẽ thực hiện lưu trữ và đọc dữ liệu từ ổ đĩa, và chúng ta sẽ xem xét cách sắp xếp dữ liệu.
* [Command line & project structure](command-line.md) - Hỗ trợ nhiều ứng dụng từ một code base và đọc input từ command line.
* [Time](time.md) - Sử dụng package `time` để lên lịch cho các hoạt động.
* [WebSockets](websockets.md) - Tìm hiểu cách viết và test một server sử dụng Websocket.

### Testing cơ bản

Nói về các chủ đề khác xung quanh việc test.

* [Giới thiệu về acceptance tests](intro-to-acceptance-tests.md) - Tìm hiểu cách viết các acceptance test cho code của bạn bằng một ví dụ thực tế về việc graceful shutdown một HTTP server.
* [Mở rộng acceptance tests](scaling-acceptance-tests.md) - Tìm hiểu các kỹ thuật để quản lý độ phức tạp của việc viết các acceptance test cho các hệ thống không tầm thường.

### Hỏi và đáp

Tôi thường tập trung vào các câu hỏi trên internet kiểu như

> Làm thế nào để tôi test hàm của tôi mà nó thực hiện x, y và z

Nếu bạn có một câu hỏi như vậy, hãy tạo một issue trên github và tôi sẽ cố gắng tìm thời gian để viết một chương nhỏ để xử lý issue đó. Tôi cảm thấy rằng nội dung như thế này là hữu ích vì nó giải quyết các câu hỏi _thật_ của con người xung quanh việc test.

* [OS exec](os-exec.md) - Một ví vụ về cách dùng hệ điều hành để chạy một lệnh lấy dữ liệu và giữ cấu trúc testable/ của chúng ta
* [Các loại error](error-types.md) - Ví dụ về cách tạo ra loại error để cải tiến test và làm cho code dễ hiểu hơn.
* [Context-aware Reader](context-aware-reader.md) - Tìm hiểu các tăng cường TDD với `io.Reader` và có thể huỷ. Dựa trên bài viết [Context-aware io.Reader for Go](https://pace.dev/blog/2020/02/03/context-aware-ioreader-for-golang-by-mat-ryer)
* [Quay lại với HTTP Handlers](http-handlers-revisited.md) - Test các HTTP handler dường như là nguyên nhân của sự tồn tại của một developer. Chương này sẽ tập trung vào việc thiết kế các handler sao cho đúng.

### Các phần khác / Thảo luận

* [Tại sao cần unit test và cách làm cho chúng làm việc cho bạn](why.md) - Xem một video, hoặc đọc một bài viết tại sao cần viết unit test và vì sao TDD là quan trọng.
* [Anti-patterns](anti-patterns.md) - Một chương ngắn về TDD và các anti-pattern khi viết unit test.

## Đóng góp cho dự án

* _Dựa án này vẫn đang phát triển_ Nếu bạn muốn đóp góp thì đừng ngại liên hệ với chúng tôi.
* Hãy đọc [contributing.md](https://github.com/quii/learn-go-with-tests/tree/842f4f24d1f1c20ba3bb23cbc376c7ca6f7ca79a/contributing.md) để biết cách đóng góp như thế nào.
* Bạn có ý tưởng gì mới? Hãy tạo một issue

## Background của tác giả

Tôi có một vài kinh nghiệm trong việc giới thiệu Go cho team phát triển, và tôi đã thử nhiều cách tiếp cận cũng như cách phát triển từ một vài người chưa hiểu rõ về Go trở thành những người viết các hệ thống bằng Go rất tốt.

### Những điều đã không khả thi

#### Đọc _một cuốn sách_

Một cách tiếp cận mà chúng tôi đã thử đó là dùng [một cuốn sách](https://www.amazon.co.uk/Programming-Language-Addison-Wesley-Professional-Computing/dp/0134190440), chúng tôi thảo luận nó mỗi tuần về các chương và bài tập trong đó.

Tôi thích cuốn sách này, nhưng nó yêu cầu dành rất nhiều thời gian cho nó. Cuốn sách này rất chi tiết về việc giải thích các khái niệm, điều này là rất tốt. Tuy nhiên nó cũng đồng nghĩa với việc quá trình tiếp thu sẽ rất chậm và nó không phù hợp với tất cả mọi người.

Tôi đã thấy chỉ một số lượng nhỏ thành viên đọc chương X và làm các bài tập, nhiều người không làm điều này.

#### Giải quyết một vài bài toán

[Katas](https://pkg.go.dev/github.com/jreisinger/gokatas#section-readme)) rất thú vị, nhưng nó thường bị giới hạn trong khuôn khổ học một ngôn ngữ; bạn hầu như không thể dùng goroutines để giải quyết kata.

Another problem is when you have varying levels of enthusiasm. Some people just learn way more of the language than others and when demonstrating what they have done end up confusing people with features the others are not familiar with.

This ends up making the learning feel quite _unstructured_ and _ad hoc_.

### What did work

By far the most effective way was by slowly introducing the fundamentals of the language by reading through [go by example](https://gobyexample.com/), exploring them with examples and discussing them as a group. This was a more interactive approach than "read chapter x for homework".

Over time the team gained a solid foundation of the _grammar_ of the language so we could then start to build systems.

This to me seems analogous to practicing scales when trying to learn guitar.

It doesn't matter how artistic you think you are, you are unlikely to write good music without understanding the fundamentals and practicing the mechanics.

### What works for me

When _I_ learn a new programming language I usually start by messing around in a REPL but eventually, I need more structure.

What I like to do is explore concepts and then solidify the ideas with tests. Tests verify the code I write is correct and documents the feature I have learned.

Taking my experience of learning with a group and my own personal way I am going to try and create something that hopefully proves useful to other teams. Learning the fundamentals by writing small tests so that you can then take your existing software design skills and ship some great systems.

## Who this is for

* People who are interested in picking up Go.
* People who already know some Go, but want to explore testing with TDD.

## What you'll need

* A computer!
* [Installed Go](https://golang.org/)
* A text editor
* Some experience with programming. Understanding of concepts like `if`, variables, functions etc.
* Comfortable with using the terminal

## Feedback

* Add issues/submit PRs [here](https://github.com/quii/learn-go-with-tests) or [tweet me @quii](https://twitter.com/quii)

[MIT license](LICENSE.md)

[Logo is by egonelbre](https://github.com/egonelbre) What a star!
