# Integers

**[Bạn có thể xem tất cả các code của chương này ở đây](https://github.com/quii/learn-go-with-tests/tree/main/integers)**

Integers (số nguyên) hoạt động như những gì bạn mong đợi. Chúng ta sẽ viết một function `Add` để thử một vài thứ. Tạo một file test có tên `adder_test.go` và thêm đoạn code này.

**Chú ý:** Các file mã nguồn Go có thể chỉ có một `package` trong mỗi thư mục. Hãy đảm bảo rằng các file của bạn được nhóm vào các package tương ứng. [Đây là một giải thích hay cho điều này.](https://dave.cheney.net/2014/12/01/five-suggestions-for-setting-up-a-go-project) 

Thư mục dự án của bạn có thể trông giống như thế này:

```
learnGoWithTests
    |
    |-> helloworld
    |    |- hello.go
    |    |- hello_test.go    
    |
    |-> integers
    |    |- adder_test.go
    |
    |- go.mod
    |- README.md
```

## Viết test trước

```go
package integers

import "testing"

func TestAdder(t *testing.T) {
	sum := Add(2, 2)
	expected := 4

	if sum != expected {
		t.Errorf("expected '%d' but got '%d'", expected, sum)
	}
}
```

Bạn sẽ thấy rằng chúng ta sử dụng `%d` để định dạng chuỗi thay vì `%q`. Điều này bởi vì chúng ta muốn in ra một số nguyên thay vì một chuỗi.

Cũng chú ý rằng chúng ta không dùng package main nữa, thay vào đó chúng ta định nghĩa một package có tên là `integers` để cho biết nó sẽ nhóm các function làm việc với interger như là `Add`.

## Thử chạy test

Thử chạy test với `go test`

Kiểm tra lỗi biên dịch

`./adder_test.go:6:9: undefined: Add`

## Viết đoạn code tối thiểu để test có thể chạy và kiểm tra thông báo fail

Viết một đoạn code vừa đủ để trình biên dịch hiểu _và chỉ vậy thôi_ - nên nhớ rằng chúng ta muốn kiểm tra test fail có đúng hay không.

```go
package integers

func Add(x, y int) int {
	return 0
}
```

Khi bạn có nhiều hơn một tham số có cùng một kiểu dữ liệu \(ở đây là hai số nguyên\), thay vì `(x int, y int)` thì bạn có thể khai báo ngắn gọn hơn là `(x, y int)`.

Bây giờ chạy test và chúng ta hài lòng rằng test thông báo đúng cái sai là gì.

`adder_test.go:10: expected '4' but got '0'`

Nếu bạn để ý thì chúng ta đã tìm hiểu về _giá trị trả về được đặt tên_ trong [phần trước](hello-world.md#lần-refactor--cuối-cùng), nhưng chúng ta không dùng ở đây. Nó thường được sử dụng khi ý nghĩa của kết quả trả về không rõ ràng trong ngữ cảnh, nhưng ở đây thì đã rõ ràng rằng function `Add` sẽ cộng hai tham số lại với nhau. Bạn có thể tham khảo [tài liệu này](https://github.com/golang/go/wiki/CodeReviewComments#named-result-parameters) để hiểu thêm chi tiết.

## Viết code vừa đủ nế nó pass

Khi tuân thủ nghiêm ngặt TDD, chúng ta bây giờ nên viết _một lượng code tối thiểu vừa đủ để test pass_. Một lập trình viên nào đó có thể làm thế này

```go
func Add(x, y int) int {
	return 4
}
```

À ha! Nó chạy, TDD hơi kỳ cục đúng không?

Chúng ta có thể viết một test khác với các số khác để làm cho test của chúng ta fail, nhưng nó kiểu như [trò chơi mèo vờn chuột](https://en.m.wikipedia.org/wiki/Cat_and_mouse).

Khi chúng ta đã quen thuộc với cú pháp Go, tôi sẽ giới thiệu một kỹ thuật gọi là *"Property Based Testing"*, nó sẽ giúp các lập trình viên khỏi bực mình và giúp bạn tìm ra bug.

Hiện tại, hãy sửa nó cho đúng đã

```go
func Add(x, y int) int {
	return x + y
}
```

Nếu bạn chạy lại test thì nó sẽ pass.

## Refactor

Không có nhiều code _thật sự_ chúng ta cần phải cải thiện ở đây.

Chúng ta đã tìm hiểu cách để đặt tên tham số trả về để nó không những hiện trong tài liệu, mà còn trong hầu hết các text editor của các lập trình viên.

Điều này là tốt bởi vì nó hỗ trợ cho việc sử dụng code khi bạn viết code. Nó cho phép người dùng có thể hiểu các dùng code của bạn chỉ bằng cách nhìn vào kiểu dữ liệu và tài liệu.

Bạn có thể thêm tài liệu cho function bằng các comment, và chúng sẽ xuất hiện trong Go Doc như khi bạn xem tài liệu của các thư viện chuẩn.

```go
// Add takes two integers and returns the sum of them.
func Add(x, y int) int {
	return x + y
}
```

### Các ví dụ

Nếu bạn muốn tìm hiểu thêm, bạn có thể làm vài [ví dụ](https://blog.golang.org/examples). Bạn sẽ thấy nhiều ví dụ ở trong tài liệu của thư viện chuẩn.

Thường thì các ví dụ có thể tìm thấy ở bên ngoài codebase, ví dụ như một file readme, sẽ thường ở nên lỗi thời và không chính xác so với code thật, bởi vì họ không kiểm tra chúng.

Các ví dụ được chạy như các test, do đó bạn có thể tự tin rằng các ví dụ phản ánh đúng những gì code thật thực hiện.

Các ví dụ đã được biên dịch \(và tùy chọn để chạy\) như là một phần của các bộ test.

Như là các test thông thường, các vị dụ là các function nằm trong một package tại các file `_test.go`  Thêm function `ExampleAdd` vào file `adder_test.go`.

```go
func ExampleAdd() {
	sum := Add(1, 5)
	fmt.Println(sum)
	// Output: 6
}
```

(Nếu editor của bạn không tự động import các package cho bạn, bước biên dịch sẽ fail bởi vì bạn thiếu `import "fmt"` trong `adder_test.go`. Tôi khuyên bạn nên tìm hiểu tại sao có các lỗi này và làm cho editor của bạn tự động thực hiện nó khi bạn dùng.)

Nếu code của bạn thay đổi thì ví dụ đó sẽ không còn đúng, và bạn build sẽ thất bại.

Chạy bộ test của package, bạn có thể thấy function ví dụ được chạy mà không cần sự can thiệp của chúng ta:

```bash
$ go test -v
=== RUN   TestAdder
--- PASS: TestAdder (0.00s)
=== RUN   ExampleAdd
--- PASS: ExampleAdd (0.00s)
```

Chú ý rằng function ví dụ sẽ không được chạy nếu bạn xóa comment `// Output: 6`. Mặc dù function này sẽ được biên dịch, nhưng nó sẽ không chạy.

Bằng cách thêm đoạn code này, ví dụ sẽ xuất hiện trong tài liệu bên trong `godoc`, làm cho code của bạn sẽ dàng tiếp cận hơn.

Để thử nó, chạy `godoc -http=:6060` và đi đến `http://localhost:6060/pkg/`

Ở đó, bạn sẽ thấy một danh sách tất các các package, và bạn sẽ thấy tài liệu của ví dụ của bạn.

Nếu bạn phát hành code của bạn với các ví dụ ra một public URL, bạn có thể chia sẻ tài liệu về code của bạn tại [pkg.go.dev](https://pkg.go.dev/). Ví dụ, [ở đây](https://pkg.go.dev/github.com/quii/learn-go-with-tests/integers/v2) là API cho chương này. Trang web này cho phép bạn tìm tài liệu của các thư viện chuẩn cũng như của các bên thứ ba.

## Tóm tắt

Chúng ta đã đề cập:

* Thực hành nhiều hơn về quy trình TDD
* Cộng số các nguyên
* Viết tài liệu cho người dùng code của chúng ta để họ hiểu và sử dụng một cách nhanh chóng
* Các ví dụ về cách sử dụng code của chúng ta là những phần được kiểm tra như một phần trong bộ test
