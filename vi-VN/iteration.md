# Vòng lặp

**[Bạn có thể xem tất cả các code của chương này ở đây](https://github.com/quii/learn-go-with-tests/tree/main/for)**

Để thực hiện các việc lặp đi lặp lại trong Go, bạn cần `for`. Go không có các từ khóa `while`, `do`, `until`, bạn chỉ có thể sử dụng `for`. Đây là một điều tốt!

Hãy viết một test cho một function để lặp lại một ký tự 5 lần.

Không có gì mới ở đây, vậy bạn hãy thử tự viết như là một bài tập.

## Viết test trước

```go
package iteration

import "testing"

func TestRepeat(t *testing.T) {
	repeated := Repeat("a")
	expected := "aaaaa"

	if repeated != expected {
		t.Errorf("expected %q but got %q", expected, repeated)
	}
}
```

## Thử chạy test

`./repeat_test.go:6:14: undefined: Repeat`

## Viết một ít code vừa đủ để test chạy và kiểm tra kết quả fail

_Hãy tuân thủ kỷ luật!_ Bạn không cần kiến thức gì mới ở đây để có thể viết một test fail.

Tất cả những gì bạn cần bây giờ đã đủ để có thể biên dịch nó và bạn có thể kiểm tra test đã viết.

```go
package iteration

func Repeat(character string) string {
	return ""
}
```

Không phải là tốt hay sao khi bạn biết Go vừa đủ để viết test cho một vài bài toán cơ bản? Điều này có nghĩa là bây giờ bạn có thể chơi đùa với code của sản phẩm bao nhiêu bạn muốn và biết về cách hoạt động của nó như bạn mong muốn.

`repeat_test.go:10: expected 'aaaaa' but got ''`

## Viết đủ code để làm cho nó pass

Cú pháp của `for` không có gì đặc biệt và giống với hầu hết ngôn ngữ giống C.

```go
func Repeat(character string) string {
	var repeated string
	for i := 0; i < 5; i++ {
		repeated = repeated + character
	}
	return repeated
}
```

Khác với các ngôn ngữ khác như C, Java, hay Javascript, ở đây không có dấu ngoặc tròn bao quanh 3 phần của câu lệnh, và bắt buộc phải luôn có cặp ngoặc nhọn `{ }`. Bạn sẽ thắc mắc là cái gì đang diễn ra ở dòng này

```go
	var repeated string
```

như chúng ta đã dùng `:=` để định nghĩa và khởi tạo các biến. Tuy nhiên, `:=` đơn giản là [ngắn gọn của hai bước](https://gobyexample.com/variables). Ở đây chúng ta chỉ đang định nghĩa nghĩa một biến `string`. Do đó, đây là phiên bản rõ ràng của nó. Chúng ta cũng có thể sử dụng `var` để định nghĩa các function, chúng ta sẽ tìm hiểu nó sau.

Chạy test và nó sẽ pass.

Các biến thể khác của vòng lặp for được mô tả ở [đây](https://gobyexample.com/for).

## Refactor

Bây giờ là thời điểm cho refactor, và giới thiệu một cấu trúc khác `+=` của toán tử gán.

```go
const repeatCount = 5

func Repeat(character string) string {
	var repeated string
	for i := 0; i < repeatCount; i++ {
		repeated += character
	}
	return repeated
}
```

`+=` được gọi là _"toán tử Cộng VÀ gán"_, cộng vế bên phải vào vế bên trái rồi gán kết quả cho vế bên trái. Nó cũng hoạt động với các kiểu dữ liệu khác như integer.

### Benchmarking

Viết [benchmark](https://golang.org/pkg/testing/#hdr-Benchmarks) là một chức năng nổi bật khác trong Go và nó rất giống như viết test.

```go
func BenchmarkRepeat(b *testing.B) {
	for i := 0; i < b.N; i++ {
		Repeat("a")
	}
}
```

Bạn sẽ thấy rằng code khá tương đồng với test.

`testing.B` cho phép bạn gọi một mã có sẵn `b.N`.

Khi code benchmark chạy, nó sẽ chạy `b.N` lần và đo thời gian nó chạy.

Số lần mà code chạy thì bạn không cần quan tâm, framwork sẽ tự xác định đâu là giá trị "tốt" và nó sẽ cho bạn biết kết quả.

Để chạy benchmark, gõ `go test -bench=.` (hoặc nếu bạn đang dùng Powershell của Windows thì là `go test -bench="."`)

```text
goos: darwin
goarch: amd64
pkg: github.com/quii/learn-go-with-tests/for/v4
10000000           136 ns/op
PASS
```

`136 ns/op` nghĩa là function của chúng ta tốn trung bình khoảng 136 nanoseconds để chạy \(trên máy tính của tôi\). Khá là tốt! Để kiểm tra thì nó đã chạy 10000000 lần.

_LƯU Ý_ mặc định thì các Benchmark sẽ chạy tuần tự.

## Thực hành các bài tập

* Thay đổi test để có thể thêm và số lần lặp của ký tự và sửa lại code
* Viết `ExampleRepeat` cho tài liệu function của bạn
* Xem qua package [strings](https://golang.org/pkg/strings). Tìm xem các function mà bạn nghĩ có thể hữu dụng và thực hành chúng bằng cách viết test như chúng ta đã biết. Dành thời gian tìm hiểu về các thư viện chuẩn.

## Tóm tắt

* Thực hành nhiều hơn về TDD
* Tìm hiểu về `for`
* Tìm hiểu về cách viết các benchmark
