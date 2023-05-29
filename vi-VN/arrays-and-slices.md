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

We already refactored `Sum` - all we did was replace arrays with slices, so no extra changes are required.
Remember that we must not neglect our test code in the refactoring stage - we can further improve our `Sum` tests.

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

It is important to question the value of your tests. It should not be a goal to
have as many tests as possible, but rather to have as much _confidence_ as
possible in your code base. Having too many tests can turn in to a real problem
and it just adds more overhead in maintenance. **Every test has a cost**.

In our case, you can see that having two tests for this function is redundant.
If it works for a slice of one size it's very likely it'll work for a slice of
any size \(within reason\).

Go's built-in testing toolkit features a [coverage tool](https://blog.golang.org/cover).
Whilst striving for 100% coverage should not be your end goal, the coverage tool can help
identify areas of your code not covered by tests. If you have been strict with TDD,
it's quite likely you'll have close to 100% coverage anyway.

Try running

`go test -cover`

You should see

```bash
PASS
coverage: 100.0% of statements
```

Now delete one of the tests and check the coverage again.

Now that we are happy we have a well-tested function you should commit your
great work before taking on the next challenge.

We need a new function called `SumAll` which will take a varying number of
slices, returning a new slice containing the totals for each slice passed in.

For example

`SumAll([]int{1,2}, []int{0,9})` would return `[]int{3, 9}`

or

`SumAll([]int{1,1,1})` would return `[]int{3}`

## Write the test first

```go
func TestSumAll(t *testing.T) {

	got := SumAll([]int{1, 2}, []int{0, 9})
	want := []int{3, 9}

	if got != want {
		t.Errorf("got %v want %v", got, want)
	}
}
```

## Try and run the test

`./sum_test.go:23:9: undefined: SumAll`

## Write the minimal amount of code for the test to run and check the failing test output

We need to define `SumAll` according to what our test wants.

Go can let you write [_variadic functions_](https://gobyexample.com/variadic-functions) that can take a variable number of arguments.

```go
func SumAll(numbersToSum ...[]int) []int {
	return nil
}
```

This is valid, but our tests still won't compile!

`./sum_test.go:26:9: invalid operation: got != want (slice can only be compared to nil)`

Go does not let you use equality operators with slices. You _could_ write
a function to iterate over each `got` and `want` slice and check their values
but for convenience sake, we can use [`reflect.DeepEqual`][deepEqual] which is
useful for seeing if _any_ two variables are the same.

```go
func TestSumAll(t *testing.T) {

	got := SumAll([]int{1, 2}, []int{0, 9})
	want := []int{3, 9}

	if !reflect.DeepEqual(got, want) {
		t.Errorf("got %v want %v", got, want)
	}
}
```

\(make sure you `import reflect` in the top of your file to have access to `DeepEqual`\)

It's important to note that `reflect.DeepEqual` is not "type safe" - the code
will compile even if you did something a bit silly. To see this in action,
temporarily change the test to:

```go
func TestSumAll(t *testing.T) {

	got := SumAll([]int{1, 2}, []int{0, 9})
	want := "bob"

	if !reflect.DeepEqual(got, want) {
		t.Errorf("got %v want %v", got, want)
	}
}
```

What we have done here is try to compare a `slice` with a `string`. This makes
no sense, but the test compiles! So while using `reflect.DeepEqual` is
a convenient way of comparing slices \(and other things\) you must be careful
when using it.

Change the test back again and run it. You should have test output like the following

`sum_test.go:30: got [] want [3 9]`

## Write enough code to make it pass

What we need to do is iterate over the varargs, calculate the sum using our
existing `Sum` function, then add it to the slice we will return

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

Lots of new things to learn!

There's a new way to create a slice. `make` allows you to create a slice with
a starting capacity of the `len` of the `numbersToSum` we need to work through.

You can index slices like arrays with `mySlice[N]` to get the value out or
assign it a new value with `=`

The tests should now pass.

## Refactor

As mentioned, slices have a capacity. If you have a slice with a capacity of
2 and try to do `mySlice[10] = 1` you will get a _runtime_ error.

However, you can use the `append` function which takes a slice and a new value,
then returns a new slice with all the items in it.

```go
func SumAll(numbersToSum ...[]int) []int {
	var sums []int
	for _, numbers := range numbersToSum {
		sums = append(sums, Sum(numbers))
	}

	return sums
}
```

In this implementation, we are worrying less about capacity. We start with an
empty slice `sums` and append to it the result of `Sum` as we work through the varargs.

Our next requirement is to change `SumAll` to `SumAllTails`, where it will
calculate the totals of the "tails" of each slice. The tail of a collection is
all items in the collection except the first one \(the "head"\).

## Write the test first

```go
func TestSumAllTails(t *testing.T) {
	got := SumAllTails([]int{1, 2}, []int{0, 9})
	want := []int{2, 9}

	if !reflect.DeepEqual(got, want) {
		t.Errorf("got %v want %v", got, want)
	}
}
```

## Try and run the test

`./sum_test.go:26:9: undefined: SumAllTails`

## Write the minimal amount of code for the test to run and check the failing test output

Rename the function to `SumAllTails` and re-run the test

`sum_test.go:30: got [3 9] want [2 9]`

## Write enough code to make it pass

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

Slices can be sliced! The syntax is `slice[low:high]`. If you omit the value on
one of the sides of the `:` it captures everything to that side of it. In our
case, we are saying "take from 1 to the end" with `numbers[1:]`. You may wish to
spend some time writing other tests around slices and experiment with the
slice operator to get more familiar with it.

## Refactor

Not a lot to refactor this time.

What do you think would happen if you passed in an empty slice into our
function? What is the "tail" of an empty slice? What happens when you tell Go to
capture all elements from `myEmptySlice[1:]`?

## Write the test first

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

## Try and run the test

```text
panic: runtime error: slice bounds out of range [recovered]
    panic: runtime error: slice bounds out of range
```

Oh no! It's important to note that while the test _has compiled_, it _has a runtime error_.  

Compile time errors are our friend because they help us write software that works,  
runtime errors are our enemies because they affect our users.

## Write enough code to make it pass

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

Our tests have some repeated code around the assertions again, so let's extract those into a function.

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

We could've created a new function `checkSums` like we normally do, but in this case, we're showing a new technique, assigning a function to a variable. It might look strange but, it's no different to assigning a variable to a `string`, or an `int`, functions in effect are values too. 

It's not shown here, but this technique can be useful when you want to bind a function to other local variables in "scope" (e.g between some `{}`). It also allows you to reduce the surface area of your API. 

By defining this function inside the test, it cannot be used by other functions in this package. Hiding variables and functions that don't need to be exported is an important design consideration.

A handy side-effect of this is this adds a little type-safety to our code. If
a developer mistakenly adds a new test with `checkSums(t, got, "dave")` the compiler
will stop them in their tracks.

```bash
$ go test
./sum_test.go:52:21: cannot use "dave" (type string) as type []int in argument to checkSums
```

## Wrapping up

We have covered

* Arrays
* Slices
  * The various ways to make them
  * How they have a _fixed_ capacity but you can create new slices from old ones
    using `append`
  * How to slice, slices!
* `len` to get the length of an array or slice
* Test coverage tool
* `reflect.DeepEqual` and why it's useful but can reduce the type-safety of your code

We've used slices and arrays with integers but they work with any other type
too, including arrays/slices themselves. So you can declare a variable of
`[][]string` if you need to.

[Check out the Go blog post on slices][blog-slice] for an in-depth look into
slices. Try writing more tests to solidify what you learn from reading it.

Another handy way to experiment with Go other than writing tests is the Go
playground. You can try most things out and you can easily share your code if
you need to ask questions. [I have made a go playground with a slice in it for you to experiment with.](https://play.golang.org/p/ICCWcRGIO68)

[Here is an example](https://play.golang.org/p/bTrRmYfNYCp) of slicing an array
and how changing the slice affects the original array; but a "copy" of the slice
will not affect the original array.
[Another example](https://play.golang.org/p/Poth8JS28sc) of why it's a good idea
to make a copy of a slice after slicing a very large slice.

[for]: ../iteration.md#
[blog-slice]: https://blog.golang.org/go-slices-usage-and-internals
[deepEqual]: https://golang.org/pkg/reflect/#DeepEqual
[slice]: https://golang.org/doc/effective_go.html#slices
