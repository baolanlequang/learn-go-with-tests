# Learn Go with Tests

<div style="text-align: center">
  <img src="../red-green-blue-gophers-smaller.png" />
</div>

[Đồ hoạ thiết kế bởi Denise](https://twitter.com/deniseyu21)
[Bản dịch tiếng Việt bởi Lê Quang Bảo Lân](https://github.com/baolanlequang/learn-go-with-tests)

## Hỗ trợ tôi

Tôi hân hạnh cung cấp miễn phí nguồn tài liệu này, nhưng nếu bạn có thể cảm ơn bằng các cách sau:

- [Theo dõi tôi trên Twitter @quii](https://twitter.com/quii)
- <a rel="me" href="https://mastodon.cloud/@quii">Mastodon</a>
- [Mua cho tôi một ly cà phê :coffee:](https://www.buymeacoffee.com/quii)
- [Tài trợ cho tôi trên GitHub](https://github.com/sponsors/quii)

## Học test-driven development với Go

* Khám phá ngôn ngữ Go bằng cách viết test
* **Xây dựng nền tảng TDD**. Go là một ngôn ngữ tốt cho việc học TDD, bởi vì nó là một ngôn ngữ đơn giản và dễ học và các thư viện dùng để test được tích hợp sẵn.
* Tự tin rằng bạn sẽ có thể bắt đầu viết các hệ thống vững chắc, được test tốt bằng Go.

Các bản dịch:

- [Tiếng Việt](https://lan-le.gitbook.io/hoc-go-bang-cach-viet-test)
- [Tiếng Trung](https://studygolang.gitbook.io/learn-go-with-tests)
- [Tiếng Bồ Đào Nha](https://larien.gitbook.io/aprenda-go-com-testes/)
- [Tiếng Nhật](https://andmorefine.gitbook.io/learn-go-with-tests/)
- [Tiếng Hàn](https://miryang.gitbook.io/learn-go-with-tests/)
- [Tiếng Thổ Nhĩ Kỳ](https://halilkocaoz.gitbook.io/go-programlama-dilini-ogren/)

## Background của tác giả

Tôi có một vài kinh nghiệm trong việc giới thiệu Go cho team phát triển, và tôi đã thử nhiều cách tiếp cận cũng như cách phát triển từ một vài người chưa hiểu rõ về Go trở thành những người viết các hệ thống bằng Go rất tốt.

### Những điều đã không khả thi

#### Đọc _một cuốn sách_

Một cách tiếp cận mà chúng tôi đã thử đó là dùng [một cuốn sách](https://www.amazon.co.uk/Programming-Language-Addison-Wesley-Professional-Computing/dp/0134190440), chúng tôi thảo luận nó mỗi tuần về các chương và bài tập trong đó

Tôi thích cuốn sách này, nhưng nó yêu cầu dành rất nhiều thời gian cho nó. Cuốn sách này rất chi tiết về việc giải thích các khái niệm, điều này là rất tốt. Tuy nhiên nó cũng đồng nghĩa với việc quá trình tiếp thu sẽ rất chậm và nó không phù hợp với tất cả mọi người.

Tôi đã thấy chỉ một số lượng nhỏ thành viên đọc chương X và làm các bài tập, nhiều người không làm điều này.

#### Giải quyết một vài vấn đề

[Katas](https://pkg.go.dev/github.com/jreisinger/gokatas#section-readme)) rất thú vị, nhưng nó thường bị giới hạn trong khuôn khổ học một ngôn ngữ; bạn hầu như không thể dùng goroutines để giải quyết kata.

Một vấn khác khi bạn có nhiều mức độ nhiệt tình khác nhau. Một vài người chỉ học nhiều về ngôn ngữ hơn các các người khác, và khi trình bày những gì họ đã làm thì dẫn đến chuyện khó hiểu cho các người khác bằng những tính năng mà những người khác đó chưa quen.

Điều này dẫn đến chuyện việc học cảm giác như _không có cấu trúc_ và khá _mông lung_.

### Những điều đã khả thi

Cho đến hiện tại, cách hiệu quả nhất đó là giới thiệu chầm chậm các khái niệm cơ bản của ngôn ngữ bằng cách đọc qua [go by example](https://gobyexample.com/), tìm hiểu chúng với các ví dụ và thảo luận nhóm về chúng. Điều này hiệu quả hơn việc "bài tập về nhà hôm nay là đọc chương x".

Qua thời gian, team có được nền tảng _ngữ pháp_ của ngôn ngữ, do đó chúng tôi có thể bắt đầu xây dựng các hệ thống.

Điều này có vẻ tương tự như việc tập luyện các âm giai (Scale) khi học guitar.

Không quan trọng là bạn nghĩ bạn có máu nghệ sĩ thế nào, bạn sẽ gần như không thể viết một tác phẩm âm nhạc tốt nếu không có nền tảng về nhạc lý.

### Những điều khả thi với tôi

Khi _tôi_ học một ngôn ngữ lập trình mới, tôi thường bắt đầu bằng cách loay hoay trong REPL (Read–eval–print loop), nhưng dần dần tôi nhận ra rằng tôi cần có cấu trúc.

Tôi thích khám phá các khái niệm, và sau đó cũng cố các ý tưởng bằng test. Test kiểm tra rằng code mà tôi viết là đúng, và lập tài liệu cho các tính năng mà tôi đã học.

Dựa trên kinh nghiệm học với một nhóm của tôi, và kinh nghiệm cá nhân mà tôi đã thử và tạo ra cái gì đó với hy vọng nó hữu ích cho các team khác, thì cách học các khái niệm cơ bản bằng cách viết các test nhỏ sẽ khiến bạn có thể dùng các kỹ năng thiết kế phần mềm để tạo nên các hệ thống tốt.

## Cái này dành cho ai

* Những ai hứng thú với Go.
* Những ai đã biết một ít về Go, nhưng muốn khám phá nhiều hơn về test với TDD.

## Bạn cần gì để học

* Một cái máy tính!
* [Cài đặt Go](https://golang.org/)
* Một text editor để viết code
* Một ít kinh nghiệm lập trình, hiểu các khái niệm như `if`, biến, hàm, vv...
* Biết dùng terminal

## Nhận xét

* Thêm một issues hay submit PRs [tại đây](https://github.com/quii/learn-go-with-tests) hoặc [tweet tôi @quii](https://twitter.com/quii)

[Giấy phép MIT](LICENSE.md)
