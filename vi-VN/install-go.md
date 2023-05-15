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
- **Extract method/function**. It is vital to be able to take a section of code and extract functions/methods
- **Rename**. You should be able to confidently rename symbols across files.
- **go fmt**. Go has an opinioned formatter called `go fmt`. Your editor should be running this on every file save.
- **Run tests**. You should be able to do any of the above and then quickly re-run your tests to ensure your refactoring hasn't broken anything.

In addition, to help you work with your code you should be able to:

- **View function signature**. You should never be unsure how to call a function in Go. Your IDE should describe a function in terms of its documentation, its parameters and what it returns.
- **View function definition**. If it's still not clear what a function does, you should be able to jump to the source code and try and figure it out yourself.
- **Find usages of a symbol**. Being able to see the context of a function being called can help your decision process when refactoring.

Mastering your tools will help you concentrate on the code and reduce context switching.

## Wrapping up

At this point you should have Go installed, an editor available and some basic tooling in place. Go has a very large ecosystem of third party products. We have identified a few useful components here. For a more complete list, see [https://awesome-go.com](https://awesome-go.com).
