# Array và slice

**[Bạn có thể xem tất cả code của chương này tại đây](https://github.com/quii/learn-go-with-tests/tree/main/arrays)**

Array (mảng) cho phép bạn lưu trữ nhiều thành phần có cùng một kiểu dữ liệu trong một biến duy nhất theo thứ tự.

Khi bạn có một array, sẽ rất thường xuyên chúng ta cho vòng lặp để duyệt qua nó. Do đó, chúng ta sẽ dùng [kiến thức mới vừa rồi là `for`] (iteration.md) để tạo một function `Sum`. `Sum` sẽ lấy tất cả các số trong array và trả về giá trị tổng.

Hãy sử dụng kỹ năng TDD của chúng ta

## Viết test trước

Tạo một thư mục mới để làm việc. Tạo một file có tên là `sum_test.go` và thêm đoạn code sau:

```go
package main

import "testing"

func TestSum(t *testing.T) {

	numbers := [5]int{1, 2, 3, 4, 5}
    
	got := Sum(numbers)
	want := 15

	if got != want {
		t.Errorf("got %d want %d given, %v", got, want, numbers)
	}
}
```

Các array có _kích thước cố định_, là thứ mà bạn định nghĩa khi khai báo biến.
Chúng ta có thể khai báo một array bằng 2 cách sau:

* \[N\]kiểu_dữ_liệu{giá_trị_1, giá_trị_2, ..., giá_trị_N} ví dụ: `numbers := [5]int{1, 2, 3, 4, 5}`
* \[...\]kiểu_dữ_liệu{giá_trị_1, giá_trị_2, ..., giá_trị_N} ví dụ: `numbers := [...]int{1, 2, 3, 4, 5}`

Nó cũng thỉnh thoảng hữu ích khi in các input (giá trị đầu vào) cho function ở trong thông báo lỗi.
Ở đó, chúng ta sẽ dùng placeholder `%v` để in giá trị "mặc định" cho định dạng chuỗi khi làm việc với array.

[Đọc thêm về định dạng chuỗi ở đây](https://golang.org/pkg/fmt/)

## Thử chạy test

Nếu bạn đã khởi tạo go mod bằng `go mod init main` thì bạn sẽ gặp một lỗi là `_testmain.go:13:2: cannot import "main"`.
Lỗi này bởi vì thông thường, package main sẽ chỉ chứa các tích hợp của các package khác và không chứa code có thể dùng unit test, 
và do đó Go sẽ không cho phép bạn import một package với tên là `main`.

Để sửa lỗi này, bạn có thể đổi tên main module trong file `go.mod` thành một tên bất kỳ khác.


Khi sửa xong lỗi trên, nếu bạn chạy `go test` thì trình biên dịch sẽ fail với một lỗi quen thuộc là `./sum_test.go:10:15: undefined: Sum`.
Bây giờ bạn có thể viết phần code có thể dùng để test.

## Viết code tối thiểu để cho test chạy và kiểm tra kết quả fail

In `sum.go`

```go
package main

func Sum(numbers [5]int) int {
	return 0
}
```

Test của bạn bây giờ fail với _một thông báo lỗi rõ ràng_

`sum_test.go:13: got 0 want 15 given, [1 2 3 4 5]`

## Viết code vừa đủ để làm cho nó pass

```go
func Sum(numbers [5]int) int {
	sum := 0
	for i := 0; i < 5; i++ {
		sum += numbers[i]
	}
	return sum
}
```

Để lấy một giá trị của một array tại một vị trí cụ thể, chỉ cần dùng cú pháp `array[index]`.
Trong trường hợp này, chúng ta sử dụng `for` để lặp 5 lần duyệt qua array và cộng mỗi phần tử vào `sum`.

## Refactor

Chúng ta giới thiệu [`range`](https://gobyexample.com/range) để làm cho code gọn gàng hơn

```go
func Sum(numbers [5]int) int {
	sum := 0
	for _, number := range numbers {
		sum += number
	}
	return sum
}
```

`range` cho phép bạn lặp qua một array. Trong mỗi lần lặp, `range` trả về hai giá trị là vị trí (index) và giá trị phần tử tại vị trí đó.
Chúng ta đang lựa chọn bỏ qua giá trị vị trí bằng cách sử dụng `_` [blank identifier](https://golang.org/doc/effective_go.html#blank).

### Array và kiểu dữ liệu của nó

Một tính chất thú vị của array đó là kích thước được gắn với loại dữ liệu của nó.
Nếu bạn thử đưa vào một giá trị `[4]int` và một function mà nó muốn là `[5]int` thì nó sẽ không biên dịch.
Vì chúng là các kiểu dữ liệu khác nhau tương tự như đưa một `string` vào một function muốn có `int`.

Bạn có thể nghĩ rằng nó khá phiền phức khi array có kích thước cố định, và hầu như bạn sẽ không sử dụng chúng!

Go có _slice_, là thứ sẽ không gắn kích thước và bạn có thể có bất kỳ kích thước nào.

Yêu cầu tiếp theo đó là tính tổng của một tập hợp có kích thước bất kỳ.

## Viết test trước

Chúng ta bây giờ sẽ dùng [kiểu slice][slice] để có thể có tập hợp với kích thước bất kỳ.
Cú pháp rất giống với array, bạn chỉ cần bỏ qua phần kích thước khi khai báo

`mySlice := []int{1,2,3}` thay vì `myArray := [3]int{1,2,3}`

```go
func TestSum(t *testing.T) {

	t.Run("collection of 5 numbers", func(t *testing.T) {
		numbers := [5]int{1, 2, 3, 4, 5}

		got := Sum(numbers)
		want := 15

		if got != want {
			t.Errorf("got %d want %d given, %v", got, want, numbers)
		}
	})

	t.Run("collection of any size", func(t *testing.T) {
		numbers := []int{1, 2, 3}

		got := Sum(numbers)
		want := 6

		if got != want {
			t.Errorf("got %d want %d given, %v", got, want, numbers)
		}
	})

}
```

## Chạy thử test

Cái này sẽ không thể biên dịch

`./sum_test.go:22:13: cannot use numbers (type []int) as type [5]int in argument to Sum`

## Viết code tối thiểu để cho test chạy và kiểm tra kết quả fail

Vấn đề ở đây đó là chúng ta có thể hoặc là

* Phá vỡ API hiện có bằng cách thay đổi tham số trong `Sum` là một slice thay vì một array. Khi chúng ta làm việc này, 
chúng ta sẽ huỷ hoại một ngày của một ai đó bởi vì các test _khác_ sẽ không thể biên dịch nữa!
* Tạo thêm một function mới

Trong trường hợp của chúng ta, không ai khác đang dùng function của chúng ta, do đó thay vì có hai function để bảo trì, chúng ta nên chỉ có một.

```go
func Sum(numbers []int) int {
	sum := 0
	for _, number := range numbers {
		sum += number
	}
	return sum
}
```

Nếu bạn thử chạy test lại thì nó vẫn sẽ không biên dịch, bạn sẽ phải thay đổi test trước để đưa vào một slice thay vì một array.

## Viết code vừa đủ để làm cho nó pass

Rõ ràng ở đây việc sửa các vấn đề của trình biên dịch là những gì chúng ta cần để test có thể pass!

## Refactor

Chúng ta đã refactor `Sum` - tất cả những gì chúng ta đã làm là thay array bằng slice, do đó không cần thay đổi gì thêm.
Nên nhớ rằng chúng ta phải không quên phần code test ở trong bước refactor - chúng ta có thể cải thiện test của `Sum`.

```go
func TestSum(t *testing.T) {

	t.Run("collection of 5 numbers", func(t *testing.T) {
		numbers := []int{1, 2, 3, 4, 5}

		got := Sum(numbers)
		want := 15

		if got != want {
			t.Errorf("got %d want %d given, %v", got, want, numbers)
		}
	})

	t.Run("collection of any size", func(t *testing.T) {
		numbers := []int{1, 2, 3}

		got := Sum(numbers)
		want := 6

		if got != want {
			t.Errorf("got %d want %d given, %v", got, want, numbers)
		}
	})

}
```

Điều quan trọng là đặt câu hỏi về giá trị của các test của bạn. Nó không phải là đặt mục tiêu có càng nhiều
test càng tốt, nhưng phải là có các nhiều _sự tự tin_ trong code của bạn càng tốt. Có quá nhiều test có thể đi đến 
một vấn đề thực tế đó là quá tải trong việc bảo trì. **Tất cả các test đều có giá của nó**.

Trong trường hợp của chúng ta, bạn có thể thấy rằng có hai test cho function này là dư thừa.
Nếu nó hoạt động trên một slice có kích thước cố định thì nó cũng sẽ hoạt động \(theo suy đoán\) trên một slice có kích thước bấy kỳ.

Bộ test mặc định của Go có một công cụ là [coverage](https://blog.golang.org/cover).
Mặc dù để có 100% coverage không phải là mục tiêu cuối cùng của bạn, công cụ coverage có thể giúp xác định vùng code mà chưa được bao phủ bởi test.
Nếu bạn tuân thủ với TDD, nó sẽ nhanh chóng giúp bạn đến gần hơn với 100% coverage.

Thử chạy

`go test -cover`

Bạn sẽ thấy

```bash
PASS
coverage: 100.0% of statements
```

Bây giờ xoá một test và kiểm tra coverage lần nữa.

Bây giờ chúng ta hài lòng với việc có một function đã test đầy đủ, bạn nên commit
code hiện tại trước khi tiếp tục với thử thách kế tiếp.

Chúng ta cần một function mới là `SumAll`, nó sẽ lấy các số trong các slice, và trả về một slice chứa
các tổng tương ứng.

Ví dụ

`SumAll([]int{1,2}, []int{0,9})` sẽ trả về `[]int{3, 9}`

hoặc

`SumAll([]int{1,1,1})` sẽ trả về `[]int{3}`

## Viết test trước

```go
func TestSumAll(t *testing.T) {

	got := SumAll([]int{1, 2}, []int{0, 9})
	want := []int{3, 9}

	if got != want {
		t.Errorf("got %v want %v", got, want)
	}
}
```

## Chạy thử test

`./sum_test.go:23:9: undefined: SumAll`

## Viết code tối thiểu để cho test chạy và kiểm tra kết quả fail

Chúng ta cần định nghĩa `SumAll` theo những gì test của chúng ta muốn.

Go có thể cho bạn viết [_variadic functions_](https://gobyexample.com/variadic-functions), nó cho phép bạn dùng một biến bao hàm nhiều tham số.
```go
func SumAll(numbersToSum ...[]int) []int {
	return nil
}
```

Cái này đúng, nhưng test sẽ không thể biên dịch!

`./sum_test.go:26:9: invalid operation: got != want (slice can only be compared to nil)`

Go không cho phép bạn dùng toán tử bằng với slice. Bạn _có thể_ viết một function để lặp qua từng `got` và `want` slice và kiểm tra các
giá trị của chúng, nhưng có một cách tiện dụng hơn, chúng ta có thể dử dụng [`reflect.DeepEqual`][deepEqual], nó dùng để kiểm tra
nếu có _bất kỳ_ hai biến nào là tương đồng.

```go
func TestSumAll(t *testing.T) {

	got := SumAll([]int{1, 2}, []int{0, 9})
	want := []int{3, 9}

	if !reflect.DeepEqual(got, want) {
		t.Errorf("got %v want %v", got, want)
	}
}
```

\(đảm bảo rằng bạn đã thêm `import reflect` ở trên đầu file để có thể dùng `DeepEqual`\)

Cần lưu ý rằng `reflect.DeepEqual` không "type safe (an toàn về kiểu dữ liệu)" - nghĩa là code vẫn sẽ được biên dịch ngay cả khi bạn
làm cái gì đó kỳ cục. Để thấy điều này, tạm thời thay đổi test thành:

```go
func TestSumAll(t *testing.T) {

	got := SumAll([]int{1, 2}, []int{0, 9})
	want := "bob"

	if !reflect.DeepEqual(got, want) {
		t.Errorf("got %v want %v", got, want)
	}
}
```

Chúng ta vừa thử so sánh một `slice` với một `chuỗi. Điều này là không đúng, nhưng test vẫn có thể biên dịch!
Do đó, bạn phải thật cẩn thận khi sử dụng `reflect.DeepEqual` để tiện lợi cho việc so sánh các slice \(và cả những cái khác nữa\).

Thay đổi test về lại như cũ và chạy nó. Bạn sẽ thấy kết quả của test sẽ như thế này

`sum_test.go:30: got [] want [3 9]`

## Viết code vừa đủ để làm cho nó pass

Điều chúng ta cần làm đó là đi qua các tham số, tính tổng bằng function `Sum` có sẵn, sau đó thì thêm kết quả vào slice
mà chúng ta sẽ trả về

```go
func SumAll(numbersToSum ...[]int) []int {
	lengthOfNumbers := len(numbersToSum)
	sums := make([]int, lengthOfNumbers)

	for i, numbers := range numbersToSum {
		sums[i] = Sum(numbers)
	}

	return sums
}
```

Nhiều thứ mới phải học quá!

Có một cách khác để tạo slice. `make` cho phép bạn tạo một slice với kích thước ban đầu là `len` của `numbersToSum` mà chúng ta cần làm việc.

Bạn có thể dùng index như trong array với `mySlice[N]` để lấy giá trị hoặc gán một giá trị mới tại đó bằng dấu `=`

Test bây giờ sẽ pass.

## Refactor

Như đã đề cập, slice có một kích thước. Nếu bạn có một slice với kích thước 2 và thử thực hiện `mySlice[10] = 1` thì bạn sẽ gặp một lỗi _runtime_.

Tuy nhiên, bạn có thể sử dụng function `append` để dùng một slice và một giá trị mới, và trả về một slice với tất cả phần tử trong đó.

```go
func SumAll(numbersToSum ...[]int) []int {
	var sums []int
	for _, numbers := range numbersToSum {
		sums = append(sums, Sum(numbers))
	}

	return sums
}
```

Trong cách thực hiện này, chúng ta không lo nhiều về kích thước nữa. Chúng ta bắt đầu bằng một slice rỗng `sums`,
và mở rộng nó với kết của của `Sum` tương ứng với các tham số.

Yêu cầu kết tiếp của chúng ta đó là thay đổi `SumAll` thành `SumAllTails`, đó là tính tổng của các `đuôi`
của mỗi slice. Đuôi là tập hợp tất cả các phần tử trong tập hợp ngoại trừ phần tử đầu tiên.

## Viết test trước

```go
func TestSumAllTails(t *testing.T) {
	got := SumAllTails([]int{1, 2}, []int{0, 9})
	want := []int{2, 9}

	if !reflect.DeepEqual(got, want) {
		t.Errorf("got %v want %v", got, want)
	}
}
```

## Chạy thử test

`./sum_test.go:26:9: undefined: SumAllTails`

## Viết code tối thiểu để cho test chạy và kiểm tra kết quả fail

Đổi tên function thành `SumAllTails` và chạy test lại

`sum_test.go:30: got [3 9] want [2 9]`

## Viết code vừa đủ để làm cho nó pass

```go
func SumAllTails(numbersToSum ...[]int) []int {
	var sums []int
	for _, numbers := range numbersToSum {
		tail := numbers[1:]
		sums = append(sums, Sum(tail))
	}

	return sums
}
```

Slice có thể được chia nhỏ ra! Cú pháp đó là `slice[bắt_đầu:kết_thúc]`. Nếu bạn bỏ giá trị một trong
hai vế của dấu hai chấm `:`, thì nó sẽ lấy tất cả các giá trị có trong slice ở bên vế đó.
Trong trường hợp của chúng ta, chúng ta đang muốn "lấy từ vị trí 1 đến cuối cùng" với `numbers[1:]`. Bạn có thể
dành thời gian để viết thêm test và trải nghiệm toán tử chia nhỏ slice và hiểm thêm về nó.

## Refactor

Không có nhiều thứ để refactor trong lần này.

Bạn nghĩ gì nếu bạn đưa một slice rỗng vào function của chúng ta? Lúc đó thì "tail" của một slice rỗng là gì?
Điều gì sẽ xảy ra nếu bạn yêu cầu Go lấy tất cả các phần tử từ `myEmptySlice[1:]`?

## Viết test trước

```go
func TestSumAllTails(t *testing.T) {

	t.Run("make the sums of some slices", func(t *testing.T) {
		got := SumAllTails([]int{1, 2}, []int{0, 9})
		want := []int{2, 9}

		if !reflect.DeepEqual(got, want) {
			t.Errorf("got %v want %v", got, want)
		}
	})

	t.Run("safely sum empty slices", func(t *testing.T) {
		got := SumAllTails([]int{}, []int{3, 4, 5})
		want := []int{0, 9}

		if !reflect.DeepEqual(got, want) {
			t.Errorf("got %v want %v", got, want)
		}
	})

}
```

## Chạy thử test

```text
panic: runtime error: slice bounds out of range [recovered]
    panic: runtime error: slice bounds out of range
```

Ồ không! Chúng ta cần chú ý rằng khi test _đã biên dịch_ và nó _có một lỗi runtime_.

Lỗi biên dịch là bạn của chúng ta bởi vì chúng giúp chúng ta viết phần mềm có thể chạy,
nhưng lỗi runtime là kẻ thù của chúng ta bởi vì chúng ảnh hưởng tới người dùng của chúng ta.

## Viết code vừa đủ để làm cho nó pass

```go
func SumAllTails(numbersToSum ...[]int) []int {
	var sums []int
	for _, numbers := range numbersToSum {
		if len(numbers) == 0 {
			sums = append(sums, 0)
		} else {
			tail := numbers[1:]
			sums = append(sums, Sum(tail))
		}
	}

	return sums
}
```

## Refactor

Test của chúng ta lại có một vài chỗ lặp lại, vì vậy chúng ta sẽ trích xuất chúng ra một function.

```go
func TestSumAllTails(t *testing.T) {

	checkSums := func(t testing.TB, got, want []int) {
		t.Helper()
		if !reflect.DeepEqual(got, want) {
			t.Errorf("got %v want %v", got, want)
		}
	}

	t.Run("make the sums of tails of", func(t *testing.T) {
		got := SumAllTails([]int{1, 2}, []int{0, 9})
		want := []int{2, 9}
		checkSums(t, got, want)
	})

	t.Run("safely sum empty slices", func(t *testing.T) {
		got := SumAllTails([]int{}, []int{3, 4, 5})
		want := []int{0, 9}
		checkSums(t, got, want)
	})

}
```

Chúng ta có thể tạo một function mới `checkSums` như chúng ta thường làm, nhưng trong trường hợp này, chúng ta dùng một kỹ thuật mới đó là gán một function vào một biến. Nó trông có vẻ khá kỳ lạ, nhưng không có gì khác với gán một biến vào một `string` hay một `int`.

Tuy không thể hiện ra ở đây, nhưng kỹ thuật này hữu dụng khi bạn muốn gán một function với một biến cục bộ trong một "scope" (ví dụ giữa các `{}`). Nó cũng cho phép bạn giảm diện tích thể hiện của API của bạn.

Bằng cách định nghĩa function này trong test, nó không thể được sử dụng bởi các function khác trong package này. Ẩn các biến và các function mà không cần thiết để xuất ra là một lưu ý quan trọng trong thiết kế.

Một tác dụng phụ hữu ích của việc này đó là thêm một type-safety vào code của chúng ta.
Nếu một lập trình viên vô tình thêm một test với `checkSums(t, got, "dave")` thì trình biên dịch sẽ ngăn họ lại.

```bash
$ go test
./sum_test.go:52:21: cannot use "dave" (type string) as type []int in argument to checkSums
```

## Tóm tắt

Chúng ta đã bàn về

* Array
* Slice
  * Các cách để tạo ra chúng
  * Cách chúng có một kích thước _cố định_, nhưng bạn có thể tạo ra một slice mới từ một slice bằng cách dùng `append`
  * Cách để chia nhỏ slice!
* `len` để lấy kích thước của một array hay slice
* Công cụ test coverage
* `reflect.DeepEqual` và tại sao nó hữu ích, nhưng có thể làm giảm type-safety trong code của bạn

Chúng ta đã dùng slice và array với số nguyên, nhưng chúng hoạt động với các kiểu dữ liệu bất kỳ khác, ngay cả chính bản thân kiểu array/slice.
So đó bạn có thể khai báo một biết của `[][]string` nếu cần.

[Xem blog của Go về slice][blog-slice] để tìm hiểu hơn về slice. Viết thêm test để thực hiện những gì bạn học được sau khi đọc nó.

Một cách thuận tiện khác để thực hành với Go đó là viết test với Go
playground. Bạn có thể thử hầu hết mọi thứ và bạn có thể dễ dàng chia sẻ code của bạn nếu bạn cần hỏi gì đó.
[Tôi đã tạo một Go playground với một slice trong đó để bạn có thể trải nghiệm.](https://play.golang.org/p/ICCWcRGIO68)

[Đây là một ví dụ](https://play.golang.org/p/bTrRmYfNYCp) về chia nhỏ một array và array gốc thay đổi thế nào khi thay đổi slice; nhưng
một "bản sao" của slice thì không ảnh hưởng để array gốc.
[Một ví dụ khác](https://play.golang.org/p/Poth8JS28sc) về việc tại sao nên tạo một bản sao của slice sau khi chia nhỏ một slice rất lớn.

[for]: ../iteration.md#
[blog-slice]: https://blog.golang.org/go-slices-usage-and-internals
[deepEqual]: https://golang.org/pkg/reflect/#DeepEqual
[slice]: https://golang.org/doc/effective_go.html#slices
