# Hello, World

**[Bạn có thể xem tất cả code của chương này tại đây](https://github.com/quii/learn-go-with-tests/tree/main/hello-world)**

Theo truyền thống thì chương trình đầu tiên khi bạn học một ngôn ngữ lập trình mới đó là [Hello, World](https://en.m.wikipedia.org/wiki/%22Hello,_World!%22_program).

- Tạo một folder tùy ý
- Tạo một file có tên `hello.go` trong folder đó và thêm đoạn code sau vào file đó

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello, world")
}
```

Chạy nó bằng cách gõ câu lệnh `go run hello.go`.

## Cách nó hoạt động

Khi bạn viết một chương trình bằng Go, bạn sẽ có một package tên là `main` bao gồm một func `main` trong đó. Các package là cách để nhóm các code Go lại với nhau.

Từ khóa `func` là cách bạn dùng để định nghĩa một function (hàm) với tên và nội dung của nó.

Với `import "fmt"`, chúng ta thêm vào một package chứa function `Println` mà chúng ta dùng để in ra dòng chữ.

## Cách để test

Làm thế nào để bạn test cái này? Lời khuyên là bạn phân chia "domain" code của bạn với thế giới bên ngoài \(side-effects\). `fmt.Println` là một side effect \(in chữ ra stdout\) và dòng chữ chúng ta gửi là "domain" của chúng ta.

Như vậy, chúng ta sẽ phân chia những điều trên để nó dễ test hơn.

```go
package main

import "fmt"

func Hello() string {
	return "Hello, world"
}

func main() {
	fmt.Println(Hello())
}
```

Chúng ta đã tạo ra thêm một funcion với `func`, nhưng lần này chúng ta đã thêm một từ khóa `string` trong khai báo. Điều này nghĩa là function sẽ trả về kết quả là một `string`.

Bây giờ tạo thêm một file mới có tên là `hello_test.go`, đây là nơi chúng ta sẽ viết một test cho function `Hello`

```go
package main

import "testing"

func TestHello(t *testing.T) {
	got := Hello()
	want := "Hello, world"

	if got != want {
		t.Errorf("got %q want %q", got, want)
	}
}
```

## Go modules?

Bước tiếp theo là chạy các test. Nhập `go test` vào terminal của bạn. Nếu các test pass, nghĩa là bạn có thể đang dùng các phiên bản trước đây của Go. Tuy nhiên, nếu bạn đang dùng Go 1.16 hoặc mới hơn, các test của bạn có thể sẽ không chạy. Thay vào đó, bạn sẽ thấy một thông báo lỗi ở terminal như thế này:

```shell
$ go test
go: cannot find main module; see 'go help modules'
```

Vấn đề là gì? Một từ thôi, đó là [modules](https://blog.golang.org/go116-module-changes). Thật may mắn khi vấn đề được xử lý một cách dễ dàng. Nhập `go mod init hello` vào terminal, nó sẽ tạo ra một file với nội dung tương tự như sau:

```
module hello

go 1.16
```

File này sẽ cho `go` biết những thông tin cần thiết về code của bạn. Nếu bạn có kế hoạch phân phối ứng dụng của bạn, bạn sẽ cần thêm nội dung về nơi mà code của bạn có thể tải về cũng như thông tin về các dependencies (các thư viện bên ngoài mà bạn dùng). Hiện tại, file module của bạn rất đơn giản, và bạn có thể để nó như vậy. Nếu bạn muốn tìm hiểu hơn về module, [bạn có thể xem trong tài liệu của Golang](https://golang.org/doc/modules/gomod-ref). Chúng ta bây giờ có thể quay trở lại với việc test và tìm hiểu Go vì các test đã có thể chạy với Go từ 1.16 trở lên.

Trong các chương tiếp theo, bạn cần chạy `go mod init SOMENAME`, với `SOMENAME` là tên của module, trong mỗi folder mới trước khi chạy các câu lệnh như `go test` hay `go build`.

## Quay trở lại với Testing

Chạy `go test` trong terminal của bạn. Nó sẽ pass! Để kiểm tra, bạn thay đổi test bằng cách đổi nội dung của chuỗi `want`.

Chú ý rằng bạn không cần phải lựa chọn giữa nhiều framework để test khác nhau rồi phải tìm cách cài đặt chúng. Tất cả những gì bạn cần đã được tích hợp sẵn trong ngôn ngữ và cú pháp cũng giống như code mà bạn sẽ viết.

### Viết test

Viết test cũng giống như viết một function, nhưng với một vài quy tắc sau:

* Nó cần ở trong một file có tên tương tự như `xxx_test.go`, với `xxx` là tên tùy ý bạn.
* Function test phải bắt đầu bằng từ `Test`
* Function test chỉ có một tham số duy nhất là `t *testing.T`
* Để sử dụng kiểu `*testing.T`, bạn cần `import "testing"`, như chúng ta đã làm với `fmt` file khác.

Hiện tại, chúng ta biết rằng `t` kiểu `*testing.T` là "hook" vào testing framework để bạn có thể làm những việc như `t.Fail()` khi bạn muốn nó fail.

Chúng ta đã đi qua một vài chủ đề mới:

#### `if`
Câu lệnh if trong Go tương tự như trong các ngôn ngữ lập trình khác.

#### Khai báo các biến

Chúng ta đã khai báo một vài biến bằng cú pháp `varName := value`, điều này giúp chúng ta tái sử dụng một vài giá trị trong test của chúng ta cho nó dễ đọc hơn.

#### `t.Errorf`

Chúng ta gọi _method_ (_phương thức_) trong `t` để có thể in ra thông báo và làm test fail. Chữ `f` đại diện cho định dạng (format), nó cho phép chúng ta tạo một chuỗi với các giá trị được thêm vào thông qua các giá trị placeholder `%q`. Khi bạn làm cho test fail, bạn sẽ hiểu rõ hơn về nó.

Bạn có thể tìm hiểu thêm về các chuỗi placeholder tại [fmt go doc](https://golang.org/pkg/fmt/#hdr-Printing). Để test thì `%q` rất hữu ích vì nó đặt các giá trị của bạn trong dấu ngoặc kép.

Chúng ta sẽ tìm hiểu sự khác biệt giữa method và function sau.

### Go doc

Một điều thuận tiện của Go nữa đó là tài liệu của nó. Bạn có thể chạy các tài liệu trên máy bạn bằng cách chạy `godoc -http :8000`. Nếu bạn vào địa chỉ [localhost:8000/pkg](http://localhost:8000/pkg) thì bạn sẽ thấy tất cả các package đã được cài đặt trên hệ thống của bạn.

Hầu hết các thư viện chuẩn đã được tài liệu hóa rất tốt đi kèm với các ví dụ. Bạn đi đến [http://localhost:8000/pkg/testing/](http://localhost:8000/pkg/testing/) sẽ thấy những gì bạn có thể dùng.

Nếu bạn không có câu lệnh `godoc`, thì có thể bạn đang sử dụng phiên bản mới hơn của Go (1.14 hoặc mới hơn) là phiên bản [không bao gồm `godoc`](https://golang.org/doc/go1.14#godoc). Bạn có thể cài đặt nó bằng `go install golang.org/x/tools/cmd/godoc@latest`.

### Hello, YOU

Bây giờ chúng ta có một test để chúng ta có thể duyệt qua phần mềm của chúng ta một cách an toàn.

Trong ví dụ vừa rồi, chúng ta đã viết test _sau_ khi viết code để bạn có một ví dụ về cách viết test và khai báo một function. Từ bây giờ, chúng ta sẽ _viết test trước_.

Yêu cầu tiếp theo đó là chúng ta cho phép chỉ định người sẽ được chào.

Chúng ta bắt đầu thực hiện yêu cầu này bằng một test. Đây là điều cơ bản trong test driven development, nó cho phép chúng ta đảm bảo rằng test của chúng ta _thật sự_ kiểm tra những gì chúng ta muốn. Khi bạn viết test sau, thì sẽ có nguy cơ rằng các test của bạn có thể sẽ tiếp tục pass ngay cả khi code của bạn thật sự không làm việc như mong muốn.

```go
package main

import "testing"

func TestHello(t *testing.T) {
	got := Hello("Chris")
	want := "Hello, Chris"

	if got != want {
		t.Errorf("got %q want %q", got, want)
	}
}
```

Bây giờ chạy `go test`, bạn sẽ gặp một lỗi biên dịch (compilation error)

```text
./hello_test.go:6:18: too many arguments in call to Hello
    have (string)
    want ()
```

Khi sử dụng một ngôn ngữ kiểu tĩnh như Go, điều quan trọng đó là _lắng nghe trình biên dịch_. Trình biên dịch hiểu cách kết hợp và hoạt động của code của bạn.

Trong trường hợp này, trình biên dịch nói với bạn điều bạn cần làm để có thể tiếp tục. Chúng ta thay đổi function `Hello` để nhận một tham số.

Thay đổi function `Hello` để nhận một tham số có kiểu string

```go
func Hello(name string) string {
	return "Hello, world"
}
```

Nếu bạn thử chạy test một lần nữa, `hello.go` sẽ không thể biên dịch bởi vì bạn chưa đưa vào một tham số. Thêm vào từ world" để nó có thể biên dịch.

```go
func main() {
	fmt.Println(Hello("world"))
}
```

Bây giờ khi bạn chạy test thì bạn sẽ thấy như thế này

```text
hello_test.go:10: got 'Hello, world' want 'Hello, Chris''
```

Chúng ta đã có thể biên dịch được chương trình, nhưng nó không đúng với yêu cầu như test thông báo.

Chúng ta làm cho test pass bằng cách sử dụng tham số name và nối nó với từ `Hello,`

```go
func Hello(name string) string {
	return "Hello, " + name
}
```

Khi bạn chạy test thì nó sẽ pass. Thông thường, tiếp sau đây là phần trong TDD gọi là _refactor_.

### Một lưu ý với source control

Tại thời điểm này, nếu bạn đang dùng source control \(điều mà bạn nên làm\), tôi sẽ `commit` code như hiện có. Chúng ta đang có một phần mềm chạy được được bảo đảm bởi test.

Tôi _sẽ không_ push lên master, bởi vì tôi sẽ refactor nó. Thật tốt khi commit tại thời điểm này phòng trường hợp bạn bị hỗn loạn khi refactor - bạn có thể trở lại với phiên bản chạy được.

Sẽ không có nhiều refactor ở đây, nhưng chúng ta có thể giới thiệu một tính năng khác của ngôn ngữ, đó là _hằng (constant)_.

### Hằng

Hằng được định nghĩa như sau

```go
const englishHelloPrefix = "Hello, "
```

Chúng ta có thể refactor code của chúng ta như sau

```go
const englishHelloPrefix = "Hello, "

func Hello(name string) string {
	return englishHelloPrefix + name
}
```

Sau khi refactor, chạy lại test và đảm bảo rằng bạn không phá vỡ bất cứ điều gì.

Thật tốt khi nghĩ về việc sử dụng các hằng để lưu giữ ý nghĩa của các giá trị, và thỉnh thoảng tăng hiệu năng.

## Hello, world... thêm lần nữa

Yêu cầu tiếp theo khi function của chúng ta được gọi với một chuỗi rỗng thì nó sẽ in ra "Hello, World" thay vì  "Hello, ".

Bắt đầu bằng viết một test fail mới

```go
func TestHello(t *testing.T) {
	t.Run("saying hello to people", func(t *testing.T) {
		got := Hello("Chris")
		want := "Hello, Chris"

		if got != want {
			t.Errorf("got %q want %q", got, want)
		}
	})
	t.Run("say 'Hello, World' when an empty string is supplied", func(t *testing.T) {
		got := Hello("")
		want := "Hello, World"

		if got != want {
			t.Errorf("got %q want %q", got, want)
		}
	})
}
```

Ở đây chúng ta giới thiệu một công cụ khác trong việc test đó là subtest. Thỉnh thoảng nó hữu dụng để nhóm các test quanh "một thứ", và có các subtest để mô tả các ngữ cảnh khác nhau.

Lợi ích của việc này đó là bạn có thể cài đặt code để có thể sử dụng nó trong các test khác.

Khi chúng ta có có một test fail, chúng ta sửa code bằng cách sử dụng `if`.

```go
const englishHelloPrefix = "Hello, "

func Hello(name string) string {
	if name == "" {
		name = "World"
	}
	return englishHelloPrefix + name
}
```

Nếu chúng ta chạy các test của chúng ta, chúng ta sẽ thấy nó đúng với yêu cầu mới và không phá vỡ các chức năng khác.

Việc quan trọng đó là các test của bạn _phải mô tả rõ ràng_ những điều mà code bạn cần làm. Nhưng ở đây có sự lặp lại code khi chúng ta kiểm tra thông báo như chúng ta mong muốn.

Refactoring không _chỉ_ dành cho code trong sản phẩm!

Bây giờ các test đang pass, chúng ta có thể refactor chúng.

```go
func TestHello(t *testing.T) {
	t.Run("saying hello to people", func(t *testing.T) {
		got := Hello("Chris")
		want := "Hello, Chris"
		assertCorrectMessage(t, got, want)
	})

	t.Run("empty string defaults to 'world'", func(t *testing.T) {
		got := Hello("")
		want := "Hello, World"
		assertCorrectMessage(t, got, want)
	})

}

func assertCorrectMessage(t testing.TB, got, want string) {
	t.Helper()
	if got != want {
		t.Errorf("got %q want %q", got, want)
	}
}
```

Chúng ta vừa làm gì xong?

Chúng ta vừa refactor các kiểm tra của chúng ta vào một function mới. Điều này giảm sự lặp lại và tăng sự dễ đọc cho test của chúng ta. Chúng ta cần đưa vào `t *testing.T` để chúng ta có thể nói code test của chúng ta trở thành fail khi cần.

Với các helper function, cách tốt nhất là dùng một `testing.TB` là một interface mà bao gồm cả `*testing.T` và `*testing.B`, như vậy bạn có thể gọi helper function từ một test hoặc một benchmark (đừng lo nếu bây giờ bạn chưa hiểu từ "interface" là gì, nó sẽ được giải thích sau).

`t.Helper()` là cần thiết để cho bộ test biết rằng method này là một helper. Bằng cách này, khi test fail thì số thứ dự dòng code được báo cáo sẽ là ở trong _function được gọi_ thay vì ở trong test helper. Điều này sẽ giúp các lập trình viên khác kiểm tra vấn đề dễ dàng hơn. Nếu bạn vẫn chưa hiểu, comment nó lại, làm cho test fail và quan sát kết quả test. Comment ở trong Go là một cách tốt để thêm thông tin vào code của bạn, hoặc trong trường hợp này là một cách nhanh để nói cho trình biên dịch bỏ qua một dòng. Bạn có thể comment dòng `t.Helper()` lại bằng cách thêm hai dấu `//` vào đầu dòng code. Bạn có thể thấy dòng đó chuyển sang màu xám hoặc một màu đó khác với màu với phần code còn lại để xác định nó là comment.

### Quay trở lại với source control

Bây giờ chúng ta hài lòng với code, tôi sẽ amend các commit trước, do đó chúng ta chỉ dùng phiên bản code bao gồm test của nó.

### Tính kỷ luật

Chúng ta nói về quy trình một lần nữa

* Viết một test
* Làm cho trình biên dịch biên dịch được
* Chạy các test, kiểm tra nếu nó fail và kiểm tra thông báo lỗi có ý nghĩa gì
* Viết code đủ để cho test pass
* Refactor

Thoạt nhìn thì nó khá tẻ nhạt, nhưng bám vững vào quy trình này là cực kỳ quan trọng.

Làm việc này không chỉ đảm bảo bạn có _các test liên quan_, nó còn đảm bảo _bạn thiết kế phần mềm tốt_ bằng cách refactor với sự bảo đảm của test.

Kiểm tra test fail rất quan trọng bởi vì nó cũng cho phép bạn thấy các thông báo lỗi như thế nào. Là một lập trình viên, rất khó để làm việc với một codebase khi các test fail mà không rõ vấn đề là gì.

Bằng cách đảm bảo test của bạn _nhanh_ và thiết lập các công cụ, việc chạy các test rất đơn giản và bạn có thể nhẹ nhàng trong việc viết code.

Nếu không viết test, bạn đang cam kết để kiểm tra thủ công code của bạn bằng cách chạy phần mềm, điều này giảm năng suất và tiêu tốn thời gian của bạn, đặc biệt là trong thời gian dài.

## Tiếp tục nào! Thêm yêu cầu nữa

Lạy trời, chúng ta có thêm các yêu cầu nữa. Bây giờ chúng ta cần một tham số thứ hai, đó là xác định ngôn ngữ của câu chào. Nếu một ngôn ngữ được đưa vào mà chúng ta không nhận ra nó, thì để mặc định nó là tiếng Anh.

We should be confident that we can use TDD to flesh out this functionality easily!

Write a test for a user passing in Spanish. Add it to the existing suite.

```go
	t.Run("in Spanish", func(t *testing.T) {
		got := Hello("Elodie", "Spanish")
		want := "Hola, Elodie"
		assertCorrectMessage(t, got, want)
	})
```

Remember not to cheat! _Test first_. When you try and run the test, the compiler _should_ complain because you are calling `Hello` with two arguments rather than one.

```text
./hello_test.go:27:19: too many arguments in call to Hello
    have (string, string)
    want (string)
```

Fix the compilation problems by adding another string argument to `Hello`

```go
func Hello(name string, language string) string {
	if name == "" {
		name = "World"
	}
	return englishHelloPrefix + name
}
```

When you try and run the test again it will complain about not passing through enough arguments to `Hello` in your other tests and in `hello.go`

```text
./hello.go:15:19: not enough arguments in call to Hello
    have (string)
    want (string, string)
```

Fix them by passing through empty strings. Now all your tests should compile _and_ pass, apart from our new scenario

```text
hello_test.go:29: got 'Hello, Elodie' want 'Hola, Elodie'
```

We can use `if` here to check the language is equal to "Spanish" and if so change the message

```go
func Hello(name string, language string) string {
	if name == "" {
		name = "World"
	}

	if language == "Spanish" {
		return "Hola, " + name
	}
	return englishHelloPrefix + name
}
```

The tests should now pass.

Now it is time to _refactor_. You should see some problems in the code, "magic" strings, some of which are repeated. Try and refactor it yourself, with every change make sure you re-run the tests to make sure your refactoring isn't breaking anything.

```go
    const spanish = "Spanish"
    const englishHelloPrefix = "Hello, "
    const spanishHelloPrefix = "Hola, "

func Hello(name string, language string) string {
	if name == "" {
		name = "World"
	}

	if language == spanish {
		return spanishHelloPrefix + name
	}
	return englishHelloPrefix + name
}
```

### French

* Write a test asserting that if you pass in `"French"` you get `"Bonjour, "`
* See it fail, check the error message is easy to read
* Do the smallest reasonable change in the code

You may have written something that looks roughly like this

```go
func Hello(name string, language string) string {
	if name == "" {
		name = "World"
	}

	if language == spanish {
		return spanishHelloPrefix + name
	}
	if language == french {
		return frenchHelloPrefix + name
	}
	return englishHelloPrefix + name
}
```

## `switch`

When you have lots of `if` statements checking a particular value it is common to use a `switch` statement instead. We can use `switch` to refactor the code to make it easier to read and more extensible if we wish to add more language support later

```go
func Hello(name string, language string) string {
	if name == "" {
		name = "World"
	}

	prefix := englishHelloPrefix

	switch language {
	case "french":
		prefix = frenchHelloPrefix
	case "spanish":
		prefix = spanishHelloPrefix
	}

	return prefix + name
}
```

Write a test to now include a greeting in the language of your choice and you should see how simple it is to extend our _amazing_ function.

### one...last...refactor?

You could argue that maybe our function is getting a little big. The simplest refactor for this would be to extract out some functionality into another function.

```go

const (
	french  = "French"
	spanish = "Spanish"
    
    englishHelloPrefix = "Hello, "
    spanishHelloPrefix = "Hola, "
    frenchHelloPrefix = "Bonjour, "
)

func Hello(name string, language string) string {
	if name == "" {
		name = "World"
	}

	return greetingPrefix(language) + name
}

func greetingPrefix(language string) (prefix string) {
	switch language {
	case french:
		prefix = frenchHelloPrefix
	case spanish:
		prefix = spanishHelloPrefix
	default:
		prefix = englishHelloPrefix
	}
	return
}
```

A few new concepts:

* In our function signature we have made a _named return value_ `(prefix string)`.
* This will create a variable called `prefix` in your function.
  * It will be assigned the "zero" value. This depends on the type, for example `int`s are 0 and for `string`s it is `""`.
    * You can return whatever it's set to by just calling `return` rather than `return prefix`.
  * This will display in the Go Doc for your function so it can make the intent of your code clearer.
* `default` in the switch case will be branched to if none of the other `case` statements match.
* The function name starts with a lowercase letter. In Go, public functions start with a capital letter and private ones start with a lowercase. We don't want the internals of our algorithm to be exposed to the world, so we made this function private.
* Also, we can group constants in a block instead of declaring them each on their own line. It's a good idea to use a line between sets of related constants for readability.

## Wrapping up

Who knew you could get so much out of `Hello, world`?

By now you should have some understanding of:

### Some of Go's syntax around

* Writing tests
* Declaring functions, with arguments and return types
* `if`, `const` and `switch`
* Declaring variables and constants

### The TDD process and _why_ the steps are important

* _Write a failing test and see it fail_ so we know we have written a _relevant_ test for our requirements and seen that it produces an _easy to understand description of the failure_
* Writing the smallest amount of code to make it pass so we know we have working software
* _Then_ refactor, backed with the safety of our tests to ensure we have well-crafted code that is easy to work with

In our case we've gone from `Hello()` to `Hello("name")`, to `Hello("name", "French")` in small, easy to understand steps.

This is of course trivial compared to "real world" software but the principles still stand. TDD is a skill that needs practice to develop, but by breaking problems down into smaller components that you can test, you will have a much easier time writing software.
