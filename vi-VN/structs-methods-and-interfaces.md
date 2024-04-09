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

Bây giờ bạn có vài hiểu biết về struct và chúng ta có thể nói về "table driven tests".

[Table driven tests](https://github.com/golang/go/wiki/TableDrivenTests) rất hữu dụng khi bạn muốn một danh sách các test case muốn test trong cùng một hoàn cảnh.

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

Cú pháp mới duy nhất ở đây đó là tạo ra một "anonymous struct", `areaTests`. Chúng ta định nghĩa một slice của các struct bằng cách sử dụng `[]struct` với hai field, `shape` và `want`. Sau đó chúng ta đưa vào slice với các case.

Tiếp đến chúng ta lặp qua chúng như chúng ta làm với các slice khác, sử dụng các field của struct để chạy test.

Bạn có thể thấy rằng nó giúp lập trình viên đưa vào một hình mới rất dễ dàng, bằng cách khai triển `Area` và thêm nó vào test case. Hơn thế nữa, nếu có một bug với `Area`, rất dễ để thêm một test case trước khi sửa nó.

Table driven tests có thể là một công cụ tuyệt vời trong bộ công cụ của bạn, nhưng cần chắc chắn rằng bạn cần các kiểu test khác nữa. Chúng tuyệt vời khi bạn muốn kiểm tra nhiều khai triển của một interface, hoặc nếu dữ liệu được đưa vào một function có nhiều yêu cầu cần phải kiểm tra.

Chúng ta thử nghiệm điều này bằng cách thêm một hình khác và test nó, đó là hình tam giác.

## Viết test trước

Thêm một test mới cho hình mới rất dễ dàng. Chỉ cần thêm `{Triangle{12, 6}, 36.0}` vào list của chúng ta.

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

## Chạy thử test

Hãy nhớ rằng hãy chạy test trước và để trình biên dịch hướng dẫn chúng ta đến cách sửa.

## Viết code tối thiểu để cho test chạy và kiểm tra kết quả fail

`./shapes_test.go:25:4: undefined: Triangle`

Chúng ta chưa định nghĩa `Triangle`

```go
type Triangle struct {
	Base   float64
	Height float64
}
```

Thử lại lần nữa

```
./shapes_test.go:25:8: cannot use Triangle literal (type Triangle) as type Shape in field value:
    Triangle does not implement Shape (missing Area method)
```

Nó cho chúng ta biết rằng chúng ta không thể sử dụng `Triangle` như là một hình bởi vì nó không có method `Area()`, do đó chúng ta thêm một khai triển trống để cho test hoạt động

```go
func (t Triangle) Area() float64 {
	return 0
}
```

Cuối cùng code sẽ được biên dịch và chúng ta có được lỗi sau

`shapes_test.go:31: got 0.00 want 36.00`

## Viết code vừa đủ để làm cho nó pass

```go
func (t Triangle) Area() float64 {
	return (t.Base * t.Height) * 0.5
}
```

Và bây giờ các test của chúng ta đã pass!

## Refactor

Khai triển của chúng ta là ổn nhưng chúng ta có thể cải thiện test của chúng ta một chút.

Khi bạn lướt qua những dòng này

```
{Rectangle{12, 6}, 72.0},
{Circle{10}, 314.1592653589793},
{Triangle{12, 6}, 36.0},
```

thì không phải ngay tức thì có thể nhận ra được những con số này đại diện cho điều gì, và bạn nên làm cho test của bạn trở nên dễ hiểu hơn.

Hiện tại bạn chỉ biết cú pháp để tạo ra các instance của các struct là `MyStruct{val1, val2}`, nhưng bạn có thể thêm tên của các field vào.

Chúng sẽ trông như thế này

```
        {shape: Rectangle{Width: 12, Height: 6}, want: 72.0},
        {shape: Circle{Radius: 10}, want: 314.1592653589793},
        {shape: Triangle{Base: 12, Height: 6}, want: 36.0},
```

Trong [Test-Driven Development by Example](https://g.co/kgs/yCzDLF), Kent Beck refactor một vài test đến một điểm để kiểm tra:

Nguyên văn

> The test speaks to us more clearly, as if it were an assertion of truth, **not a sequence of operations**

(phần nhấn mạnh tô đậm là của tôi)

Tạm dịch tiếng Việt bởi người dịch

> Test cho chúng ta sự rõ ràng hơn về việc chúng ta đang kiểm tra tính đúng đắn, chứ không phải là một chuỗi các lệnh.

Bây giờ test của chúng ta, thay vì là một danh sách các test case, thực sự là đang kiểm tra các hình và diện tích của chúng.

## Làm cho giá trị xuất ra của test của bạn trở nên hữu dụng

Ở  phần trên khi chúng ta đang khai triển `Triangle` và chúng ta có test fail. Nó đã in ra `shapes_test.go:31: got 0.00 want 36.00`.

Chúng ta đã biết rằng nó liên quan đến `Triangle` bởi vì chúng ta đang làm việc với nó. Nhưng nếu một bug xảy ra trong hệ thống với một trong 20 test case thì sao? Làm cách nào một lập trình viên biết test case nào fail? Đây không phải là một trải nghiệm hay cho lập trình viên, họ sẽ phải tự tìm xem test case nào đã fail một cách thủ công.

Chúng ta c1 thể thay đổi thông báo lỗi thành `%#v got %g want %g`. Giá trị `%#v` định dạng chuỗi sẽ in ra struct của chúng ta cùng với giá trị trong các field của nó, nhờ vậy lập trình viên có thể thấy các thuộc tính đang được test.

Để tăng tính dễ đọc hơn cho các test case, chúng ta có thể thay đổi `want` thành một cái gì đó dễ hiểu hơn như `hasArea`.

Một cách nữa với  table driven test đó là sử dụng `t.Run` và đặt tên cho các test case.

Bằng cách gói từng test case vào `t.Run`, bạn có thể có giá trị xuất ra của test rõ ràng hơn về lý do fail vì nó sẽ in ra tên của test case.

```
--- FAIL: TestArea (0.00s)
    --- FAIL: TestArea/Rectangle (0.00s)
        shapes_test.go:33: main.Rectangle{Width:12, Height:6} got 72.00 want 72.10
```

Và bạn có thể chạy các test cụ thể bằng `go test -run TestArea/Rectangle`.

Đây là đoạn code cho test của chúng ta

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

## Tóm tắt

Bằng cách thực hành nhiều TDD hơn, chúng ta đã đi qua các giải pháp cho các bài toán toán học cơ bản và tìm hiểu thêm về các tính năng mới của ngôn ngữ bằng cách sử dụng test.&#x20;

* Khai báo các struct để tạo ra các data type cho phép bạn gom các data liên quan nhau vào một nơi và tạo nên code rõ ràng hơn.
* Khai báo các interface để bạn có thể định nghĩa các funtion có thể sử dụng bởi các kiểu khác nhau ([parametric polymorphism](https://en.wikipedia.org/wiki/Parametric\_polymorphism))
* Thêm các method để bạn có thể thêm các chức năng cho kiểu dữ liệu của bạn và bạn có thể khai triển với interface.
* Table driven tests để bạn kiểm tra dễ hơn và bộ test của bạn dễ dàng hơn để mở rộng và bảo trì

Đây là một chương quan trọng bởi vì từ đây chúng ta sẽ bắt đầu định nghĩa các kiểu dữ liệu của chúng ta. Trong các ngôn ngữ statically typed như Go, khả năng tự thiết kế kiểu dữ liệu là quan trọng để xây dựng phần mềm giúp nó sẽ hiểu, dễ ghép lại với nhau và dễ test.

Interface là một công cụ tuyệt vời để ẩn đi sự phức tạp khỏi các phần khác của hệ thống. Trong trường hợp của chúng ta, phần _code_ của test helper không cần phải biết chính xác hình nào đang được kiểm tra, nó chỉ "hỏi" về diện tích của hình đó.

Khi bạn quen thuộc với Go hơn, bạn sẽ bắt đầu nhận ra sức mạnh của các interface và các thư viện chuẩn. Bạn sẽ tìm hiểu về các interface được định nghĩa trong các thư viện chuẩn mà được sử dụng _khắp nơi_, và bằng cách khai triển nó trong kiểu dữ liệu của bạn, bạn có thể nhanh chống có được các tính năng tuyệt vời.
