# Struct, method & interface

[**Bạn có thể xem tất cả code của chương này tại đây**](https://github.com/quii/learn-go-with-tests/tree/main/structs)

Giả sử rằng chúng ta cần một đoạn code tính toán hình học để tính chu vi của một hình chữ nhật với chiều dài và chiều rộng cho trước. Chúng ta có thể viết một function `Perimeter(width float64, height float64)`, trong đó `float64` là các số floating-point ví dụ như `123.45`.

Quy trình TDD hiện tại đã khá quen thuộc với chúng ta.

## Viết test trước

```go
func TestPerimeter(t *testing.T) {
	got := Perimeter(10.0, 10.0)
	want := 40.0

	if got != want {
		t.Errorf("got %.2f want %.2f", got, want)
	}
}
```

Bạn có nhận ra cách định dạng chuỗi khác không? Chữ `f` đại diện cho `float64` và `.2` nghĩa là in ra 2 giá trị thập phân.

## Thử chạy test

`./shapes_test.go:6:9: undefined: Perimeter`

## Viết code tối thiểu để cho test chạy và kiểm tra kết quả fail

```go
func Perimeter(width float64, height float64) float64 {
	return 0
}
```

Results in `shapes_test.go:10: got 0.00 want 40.00`.

Nhận được kết quả `shapes_test.go:10: got 0.00 want 40.00`.

## Viết code vừa đủ để làm cho nó pass

```go
func Perimeter(width float64, height float64) float64 {
	return 2 * (width + height)
}
```

Cho tới hiện tại là khá dễ dàng. Bây giờ chúng ta tạo một function có tên `Area(width, height float64)` để tính diện tích của một hình chữ nhật.

Bạn hãy tự mình viết, nhớ tuân thủ quy trình TDD.

Bạn sẽ có được các test như thế này

```go
func TestPerimeter(t *testing.T) {
	got := Perimeter(10.0, 10.0)
	want := 40.0

	if got != want {
		t.Errorf("got %.2f want %.2f", got, want)
	}
}

func TestArea(t *testing.T) {
	got := Area(12.0, 6.0)
	want := 72.0

	if got != want {
		t.Errorf("got %.2f want %.2f", got, want)
	}
}
```

Và code như sau

```go
func Perimeter(width float64, height float64) float64 {
	return 2 * (width + height)
}

func Area(width float64, height float64) float64 {
	return width * height
}
```

## Refactor

Đoạn code của chúng ta thực hiện tốt công việc của nó, nhưng không có nghĩa là nó chứa đầy đủ thông tin về các hình chữ nhật. Một lập trình viên thiếu thận trọng có thể đưa vào chiều rộng và chiều cao của một hình tam giác vào những function này mà không nhận ra rằng họ có thể có kết quả sai.

Chúng ta có thể đặt tên các function một cách cụ thể hơn như `RectangleArea`. Một cách hay hơn đó là định nghĩa _type_ của chúng ta là `Rectangle` để đóng gói khái niệm này cho chúng ta.

Chúng ta có thể tạo một type bằng cách dùng một **struct.** [Struct](https://go.dev/ref/spec#Struct\_types) là tên một tập hợp các field mà bạn có thể lưu trữ dữ liệu.

Khai báo một struct sẽ như sau

```go
type Rectangle struct {
	Width  float64
	Height float64
}
```

Bây giờ chúng ta refactor các test để sử dụng `Rectangle` thay vì chỉ là các giá trị `float64`.

```go
func TestPerimeter(t *testing.T) {
	rectangle := Rectangle{10.0, 10.0}
	got := Perimeter(rectangle)
	want := 40.0

	if got != want {
		t.Errorf("got %.2f want %.2f", got, want)
	}
}

func TestArea(t *testing.T) {
	rectangle := Rectangle{12.0, 6.0}
	got := Area(rectangle)
	want := 72.0

	if got != want {
		t.Errorf("got %.2f want %.2f", got, want)
	}
}
```

Nhớ rằng chạy các test của bạn trước khi thực hiện sửa. Các test sẽ hiện ra một lỗi hữu ích như sau

```
./shapes_test.go:7:18: not enough arguments in call to Perimeter
    have (Rectangle)
    want (float64, float64)
```

Bạn có thể truy cập các field của một struct bằng cú pháp `myStruct.field`.

Thay đổi hai function để sửa test.

```go
func Perimeter(rectangle Rectangle) float64 {
	return 2 * (rectangle.Width + rectangle.Height)
}

func Area(rectangle Rectangle) float64 {
	return rectangle.Width * rectangle.Height
}
```

Tôi hy vọng rằng truyền một `Rectangle` vào một function sẽ truyền tải ý nghĩa của nó rõ ràng hơn, nhưng còn có nhiều lợi ích khác khi sử dụng struct mà chúng ta sẽ đề cập sau.

Yêu cầu tiếp theo của chúng ta đó là viết một function `Area` cho các hình tròn.

## Viết test trước

```go
func TestArea(t *testing.T) {

	t.Run("rectangles", func(t *testing.T) {
		rectangle := Rectangle{12, 6}
		got := Area(rectangle)
		want := 72.0

		if got != want {
			t.Errorf("got %g want %g", got, want)
		}
	})

	t.Run("circles", func(t *testing.T) {
		circle := Circle{10}
		got := Area(circle)
		want := 314.1592653589793

		if got != want {
			t.Errorf("got %g want %g", got, want)
		}
	})

}
```

Như bạn thấy, chữ `f` được thay bằng `g`, với lý do của nó. Sử dụng `g` sẽ in ra số thập phân cụ thể hơn trong dòng lỗi ([các tùy chọn của fmt](https://pkg.go.dev/fmt)). Ví dụ, sử dụng một bán kính là 1.5 trong tinh diện tích hình tròn, `f` sẽ hiển thị `7.068583`trong khi `g` sẽ hiển thị `7.0685834705770345`.

## Chạy thử test

`./shapes_test.go:28:13: undefined: Circle`

## Viết code tối thiểu để cho test chạy và kiểm tra kết quả fail

Chúng cần định nghĩa type `Circle`.

```go
type Circle struct {
	Radius float64
}
```

Bây giờ thử chạy test một lần nữa

`./shapes_test.go:29:14: cannot use circle (type Circle) as type Rectangle in argument to Area`

Một số ngôn ngữ lập trình cho phép bạn có thể làm như sau:

```go
func Area(circle Circle) float64       {}
func Area(rectangle Rectangle) float64 {}
```

Nhưng bạn không thể làm với Go

`./shapes.go:20:32: Area redeclared in this block`

Chúng ta có hai lựa chọn:

* Bạn có thể có các function có chung tên được định nghĩa ở các _package_ khác nhau. Do đó chúng ta có thể tạo `Area(Circle)` trong một package mới, nhưng làm vậy ở đây thì hơi quá.
* Chúng ta có thể định nghĩa các [method](https://go.dev/ref/spec#Method\_declarations) trong type mới này.

### Các method là gì?

Hiện tại chúng ta chỉ biết viết các _function_ nhưng chúng ta đã dùng một vài method. Khi chúng ta gọi `t.Errorf`, chúng ta đang gọi method `Errorf` của instance `t` (`testing.T`).

Một method là một function với một receiver. Một khai báo method liên kết một định danh, là tên của nó, và liên kết nó với type cơ bản của receiver.

Các method rất giống với các function, nhưng chúng được gọi bằng cách gọi thông qua một instance của một type cụ thể. Trong khi đó bạn có thể gọi các function ở bất cứ đâu bạn muốn, như `Area(rectangle)` bạn chỉ có thể gọi các method thông qua "một sự vật".

Một ví dụ sẽ giúp hiểu rõ hơn, chúng ta thay đổi các test trước để gọi các method và sau đó sửa code.

```go
func TestArea(t *testing.T) {

	t.Run("rectangles", func(t *testing.T) {
		rectangle := Rectangle{12, 6}
		got := rectangle.Area()
		want := 72.0

		if got != want {
			t.Errorf("got %g want %g", got, want)
		}
	})

	t.Run("circles", func(t *testing.T) {
		circle := Circle{10}
		got := circle.Area()
		want := 314.1592653589793

		if got != want {
			t.Errorf("got %g want %g", got, want)
		}
	})

}
```

Nếu chúng ta chạy test, chúng ta có được

```
./shapes_test.go:19:19: rectangle.Area undefined (type Rectangle has no field or method Area)
./shapes_test.go:29:16: circle.Area undefined (type Circle has no field or method Area)
```

> type Circle has no field or method Area

Tôi muốn nhắc lại ở đây là trình biên dịch tuyệt vời như thế nào. Điều quan trọng là bạn cần dành thời gian đọc các thông báo lỗi một cách cẩn thận, nó sẽ giúp ích cho bạn sau này.

## Viết code tối thiểu để cho test chạy và kiểm tra kết quả fail

Thêm vào một vài method cho các type mà chúng ta có

```go
type Rectangle struct {
	Width  float64
	Height float64
}

func (r Rectangle) Area() float64 {
	return 0
}

type Circle struct {
	Radius float64
}

func (c Circle) Area() float64 {
	return 0
}
```

Cú pháp để khai báo các method hầu hết là tương tự với các function bởi vì chúng rất giống nhau. Sự khác nhau duy nhất đó là cú pháp của method receiver là `func (receiverName ReceiverType) MethodName(args)`.

Khi method của bạn được gọi trên một biến kiểu như vậy, bạn có được tham chiếu đến dữ liệu của nó thông qua biến `receiverName`. Trong nhiều ngôn ngữ lập trình khác, việc này là ngầm định và bạn truy cập receiver thông qua `this.`

Quy ước trong Go là biến receiver là ký tự đầu tiên của type.

```
r Rectangle
```

Nếu bạn thử chạy test lại, chúng được biên dịch và sẽ cho ra một vài giá trị sai.

## Viết code vừa đủ để làm cho nó pass

Bây giờ làm cho các test của các hình chữ nhật pass bằng các method

```go
func (r Rectangle) Area() float64 {
	return r.Width * r.Height
}
```

Nếu bạn chạy lại test thì các test cho hình chữ nhật sẽ pass, nhưng các hình tròn thì vẫn fail.

Để cho các function `Area` của các hình tròn pass, chúng ta sẽ dùng hằng số `Pi` từ package `math` (nhớ rằng phải import nó).

```go
func (c Circle) Area() float64 {
	return math.Pi * c.Radius * c.Radius
}
```

## Refactor

Có vài trùng lặp trong test của chúng ta.

Chúng ta muốn là dùng một tập hợp các _hình_, gọi method `Area()` trên chúng và sau đó kiểm tra kết quả.

Chúng ta muốn có thể viết một loại function kiểu như `checkArea` mà chúng ta có thể đưa vào cả `Rectangle` và `Circle`, nhưng không thể biên dịch nếu chúng ta cố đưa vào một loại không phải kiểu hình đó.

Với Go, bạn có làm điều này với các **interface**.

[Interface](https://golang.org/ref/spec#Interface\_types) là một khái niệm rất mạnh mẽ trong các ngôn ngữ statically typed như Go bởi vì chúng cho phép bạn tạo ra các function có thể được sử dụng với nhiều type khác nhau, và tạo ra các đoạn code có tính decoupled cao trong khi vẫn duy trì type-safety.

Chúng ta hãy xem nó bằng các refactor test.

```go
func TestArea(t *testing.T) {

	checkArea := func(t testing.TB, shape Shape, want float64) {
		t.Helper()
		got := shape.Area()
		if got != want {
			t.Errorf("got %g want %g", got, want)
		}
	}

	t.Run("rectangles", func(t *testing.T) {
		rectangle := Rectangle{12, 6}
		checkArea(t, rectangle, 72.0)
	})

	t.Run("circles", func(t *testing.T) {
		circle := Circle{10}
		checkArea(t, circle, 314.1592653589793)
	})

}
```

Chúng ta tạo ra một function helper như chúng ta có trong các bài khác, nhưng lần này chúng ta đưa vào một `Shape`. Nếu chúng ta thử gọi nó bằng một cái gì đó không phải là shape, nó sẽ không thể biên dịch.

Làm thế nào để một thứ gì đó trở thành một shape? Chúng ta chỉ cần cho Go biết `Shape` là gì bằng cách khai báo một interface.

```go
type Shape interface {
	Area() float64
}
```

Chúng ta tạo ra một `type` như chúng ta đã làm với `Rectangle` và `Circle`, nhưng lần này nó là một `interface` thay vì một `struct`.

Khi bạn thêm đoạn code này, các test sẽ pass.

### Chờ chút, cái gì vậy?

Chỗ này khá khác biệt với interface ở hầu hết các ngôn ngữ khác. Thông thường bạn sẽ phải viết code như là `My type Foo implements interface Bar`.

Nhưng trong trường hợp của chúng ta&#x20;

But in our case

* `Rectangle` có một method có tên `Area` trả về một giá trị `float64` do đó nó thỏa điều kiện của interface `Shape`.
* `Circle` có một method có tên `Area` trả về một giá trị `float64` do đó nó thỏa điều kiện của interface `Shape`.
* `string` không có method đó, do đó nó không thỏa điều kiện của interface.
* vân vân.

Trong Go, **xử lý interface là ngầm định**. Nếu type của bạn đưa vào khớp những gì interface yêu cầu, nó sẽ được biên dịch.

### Decoupling

Chú ý rằng cách mà helper của chúng ta không cần quan tâm chính nó là shape kiểu `Rectangle` hay `Circle` hay `Triangle`. Bằng cách định nghĩa một interface, helper được decouple từ các type cụ thể và chỉ có method cần thiết để thực hiện công việc của nó.

Cách tiếp cận bằng cách sử dụng các interface này để khai báo **chỉ những gì bạn cần** rất là quan trọng trong thiết kế phần mềm và sẽ được đề cập chi tiết hơn trong các phần sau.

## Refactor thêm nữa

Now that you have some understanding of structs we can introduce "table driven tests".

[Table driven tests](https://github.com/golang/go/wiki/TableDrivenTests) are useful when you want to build a list of test cases that can be tested in the same manner.

```go
func TestArea(t *testing.T) {

	areaTests := []struct {
		shape Shape
		want  float64
	}{
		{Rectangle{12, 6}, 72.0},
		{Circle{10}, 314.1592653589793},
	}

	for _, tt := range areaTests {
		got := tt.shape.Area()
		if got != tt.want {
			t.Errorf("got %g want %g", got, tt.want)
		}
	}

}
```

The only new syntax here is creating an "anonymous struct", `areaTests`. We are declaring a slice of structs by using `[]struct` with two fields, the `shape` and the `want`. Then we fill the slice with cases.

We then iterate over them just like we do any other slice, using the struct fields to run our tests.

You can see how it would be very easy for a developer to introduce a new shape, implement `Area` and then add it to the test cases. In addition, if a bug is found with `Area` it is very easy to add a new test case to exercise it before fixing it.

Table driven tests can be a great item in your toolbox, but be sure that you have a need for the extra noise in the tests. They are a great fit when you wish to test various implementations of an interface, or if the data being passed in to a function has lots of different requirements that need testing.

Let's demonstrate all this by adding another shape and testing it; a triangle.

## Write the test first

Adding a new test for our new shape is very easy. Just add `{Triangle{12, 6}, 36.0},` to our list.

```go
func TestArea(t *testing.T) {

	areaTests := []struct {
		shape Shape
		want  float64
	}{
		{Rectangle{12, 6}, 72.0},
		{Circle{10}, 314.1592653589793},
		{Triangle{12, 6}, 36.0},
	}

	for _, tt := range areaTests {
		got := tt.shape.Area()
		if got != tt.want {
			t.Errorf("got %g want %g", got, tt.want)
		}
	}

}
```

## Try to run the test

Remember, keep trying to run the test and let the compiler guide you toward a solution.

## Write the minimal amount of code for the test to run and check the failing test output

`./shapes_test.go:25:4: undefined: Triangle`

We have not defined `Triangle` yet

```go
type Triangle struct {
	Base   float64
	Height float64
}
```

Try again

```
./shapes_test.go:25:8: cannot use Triangle literal (type Triangle) as type Shape in field value:
    Triangle does not implement Shape (missing Area method)
```

It's telling us we cannot use a `Triangle` as a shape because it does not have an `Area()` method, so add an empty implementation to get the test working

```go
func (t Triangle) Area() float64 {
	return 0
}
```

Finally the code compiles and we get our error

`shapes_test.go:31: got 0.00 want 36.00`

## Write enough code to make it pass

```go
func (t Triangle) Area() float64 {
	return (t.Base * t.Height) * 0.5
}
```

And our tests pass!

## Refactor

Again, the implementation is fine but our tests could do with some improvement.

When you scan this

```
{Rectangle{12, 6}, 72.0},
{Circle{10}, 314.1592653589793},
{Triangle{12, 6}, 36.0},
```

It's not immediately clear what all the numbers represent and you should be aiming for your tests to be easily understood.

So far you've only been shown syntax for creating instances of structs `MyStruct{val1, val2}` but you can optionally name the fields.

Let's see what it looks like

```
        {shape: Rectangle{Width: 12, Height: 6}, want: 72.0},
        {shape: Circle{Radius: 10}, want: 314.1592653589793},
        {shape: Triangle{Base: 12, Height: 6}, want: 36.0},
```

In [Test-Driven Development by Example](https://g.co/kgs/yCzDLF) Kent Beck refactors some tests to a point and asserts:

> The test speaks to us more clearly, as if it were an assertion of truth, **not a sequence of operations**

(emphasis in the quote is mine)

Now our tests - rather, the list of test cases - make assertions of truth about shapes and their areas.

## Make sure your test output is helpful

Remember earlier when we were implementing `Triangle` and we had the failing test? It printed `shapes_test.go:31: got 0.00 want 36.00`.

We knew this was in relation to `Triangle` because we were just working with it. But what if a bug slipped in to the system in one of 20 cases in the table? How would a developer know which case failed? This is not a great experience for the developer, they will have to manually look through the cases to find out which case actually failed.

We can change our error message into `%#v got %g want %g`. The `%#v` format string will print out our struct with the values in its field, so the developer can see at a glance the properties that are being tested.

To increase the readability of our test cases further, we can rename the `want` field into something more descriptive like `hasArea`.

One final tip with table driven tests is to use `t.Run` and to name the test cases.

By wrapping each case in a `t.Run` you will have clearer test output on failures as it will print the name of the case

```
--- FAIL: TestArea (0.00s)
    --- FAIL: TestArea/Rectangle (0.00s)
        shapes_test.go:33: main.Rectangle{Width:12, Height:6} got 72.00 want 72.10
```

And you can run specific tests within your table with `go test -run TestArea/Rectangle`.

Here is our final test code which captures this

```go
func TestArea(t *testing.T) {

	areaTests := []struct {
		name    string
		shape   Shape
		hasArea float64
	}{
		{name: "Rectangle", shape: Rectangle{Width: 12, Height: 6}, hasArea: 72.0},
		{name: "Circle", shape: Circle{Radius: 10}, hasArea: 314.1592653589793},
		{name: "Triangle", shape: Triangle{Base: 12, Height: 6}, hasArea: 36.0},
	}

	for _, tt := range areaTests {
		// using tt.name from the case to use it as the `t.Run` test name
		t.Run(tt.name, func(t *testing.T) {
			got := tt.shape.Area()
			if got != tt.hasArea {
				t.Errorf("%#v got %g want %g", tt.shape, got, tt.hasArea)
			}
		})

	}

}
```

## Wrapping up

This was more TDD practice, iterating over our solutions to basic mathematic problems and learning new language features motivated by our tests.

* Declaring structs to create your own data types which lets you bundle related data together and make the intent of your code clearer
* Declaring interfaces so you can define functions that can be used by different types ([parametric polymorphism](https://en.wikipedia.org/wiki/Parametric\_polymorphism))
* Adding methods so you can add functionality to your data types and so you can implement interfaces
* Table driven tests to make your assertions clearer and your test suites easier to extend & maintain

This was an important chapter because we are now starting to define our own types. In statically typed languages like Go, being able to design your own types is essential for building software that is easy to understand, to piece together and to test.

Interfaces are a great tool for hiding complexity away from other parts of the system. In our case our test helper _code_ did not need to know the exact shape it was asserting on, only how to "ask" for its area.

As you become more familiar with Go you will start to see the real strength of interfaces and the standard library. You'll learn about interfaces defined in the standard library that are used _everywhere_ and by implementing them against your own types, you can very quickly re-use a lot of great functionality.
