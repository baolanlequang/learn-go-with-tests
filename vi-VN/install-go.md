# Cài đặt Go, cài đặt môi trường cho ứng dụng

Tài liệu hướng dẫn cài đặt chính thức cho Go là ở [đây](https://golang.org/doc/install).

## Go Environment

### Go Modules
Bắt đầu từ Go 1.11 đã giới thiệu [Modules](https://github.com/golang/go/wiki/Modules). Cách này được mặc định từ Go 1.16, do đó việc sử dụng `GOPATH` không được khuyến khích.

Modules tập trung giải quyết các vấn đề liên quan đến quản lý dependency, lựa chọn phiên bản và build lại; nó cungg4 cho phép người sử dụng chạy code Go bên ngoài `GOPATH`.

Sử dụng Modules rất dễ dàng. Chọn bất kỳ thư mục nào ngoài `GOPATH` là thư mục gốc cho dự án của bạn, và tạo một module mới bằng câu lệnh `go mod init`.

Một file `go.mod` sẽ được sinh ra, nó chứa đường dẫn của module, thông tin phiên bản Go, và các yêu cầu dependency của nó, tức là các module khác mà nó cần để build.

Nếu không có `<modulepath>` nào được đề cập, `go mod init` sẽ thử đoán xem đường dẫn của module từ cấu trúc thư mục. Nó cũng có thể được ghi đè lên bằng cách cung cấp một tham số.

```sh
mkdir my-project
cd my-project
go mod init <modulepath>
```

Một file `go.mod` có thể trông như thế này:

```
module cmd

go 1.16

```

Tài liệu có sẵn cung cấp một cái nhìn tổng quan những gì có thể làm với câu lệnh `go mod`.

```sh
go help mod
go help mod init
```

## Go Linting

Một cải tiến so với linter mặc định đó là [GolangCI-Lint](https://golangci-lint.run).

Bạn có thể cài đặt nó như sau:

```sh
brew install golangci-lint
```

## Refactoring và các công cụ của bạn

Một phần quan trọng lớn của cuốn sách này đó là refactoring.

Các công cụ có thể giúp bạn refactor một cách tự tin hơn.

Bạn nên tìm hiểu hoặc làm quen với text editor của bạn để thực hiện các tổ hợp phím đơn giản:

- **Extract/Inline variable**. Có khả năng dùng các giá trị và tên của chúng để bạn có thể viết code nhanh hơn..
- **Extract method/function**. Thật tiện dụng nếu có thể lấy một đoạn code và trích xuất nó thành các functions/methods
- **Rename**. Bạn nên có khả năng tự tin thay đổi tên các thành phần trong các file.
- **go fmt**. Go có một định dạng được gọi là `go fmt`. Code editor của bạn nên có thể chạy câu lệnh này khi lưu tất cả các file.
- **Run tests**. Bạn nên có khả năng thực hiện các phần trên và nhanh chóng chạy lại các test để đảm bảo rằng các refactoring của bạn không phá vỡ bất cứ thứ gì.

Thêm vào đó, để giúp bạn code tốt hơn thì bạn nên có khả năng:

- **Xem dấu hiệu một hàm (function)**. Bạn nên chắc chắn rằng bạn không bao giờ không biết cách gọi một hàm. IDE của bạn sẽ mô tả một hàm về chức năng, các tham số và kết quả trả về của nó.
- **Xem định nghĩa hàm**. Nếu không rõ về một hàm sẽ làm gì, bạn nên có khả năng nhảy đến đoạn code của hàm đó và tự tìm hiểu.
- **Tìm chỗ sử dụng một biểu tượng nào đó**. Có khả năng hiểu ngữ cảnh của một hàm được gọi có thể giúp bạn quyết định khi refactor.

Nắm rõ các công cụ của bạn sẽ giúp bạn tập trung vào code và giảm thiểu việc chuyển ngữ cảnh.

## Tóm tắt

Bây giờ bạn đã cài đặt Go, có một editor và một vài công cụ cơ bản. Go có một hệ sinh thái rất lớn với nhiều sản phẩm của bên thứ ba. Chúng ta đã nêu ra một vài cái hữu dụng ở đây. Bạn vào [https://awesome-go.com](https://awesome-go.com) để xem danh sách đầy đủ.
