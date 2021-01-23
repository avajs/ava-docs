# Gỡ lỗi cho các test với WebStorm

Các bản dịch: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/debugging-with-webstorm.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/recipes/debugging-with-webstorm.md)

Bắt đầu từ phiên bản 2016.2, [WebStorm](https://www.jetbrains.com/webstorm/) và các IDE của JetBrains (IntelliJ IDEA Ultimate, PHPStorm, PyCharm Professional, và RubyMine với plugin Node.js đã được cài đặt) cho phép bạn gỡ lỗi các AVA test.


## Cài đặt dùng Node.js

Thêm một cấu hình *Node.js Run/Debug*; chọn `Edit Configurations...` từ danh sách thả xuống ở phía trên bên phải, sau đó nhấp vào `+` và chọn *Node.js*.

Trong trường `JavaScript file`, chỉ định đường dẫn đến AVA trong thư mục `node_modules` của dự án: `node_modules/.bin/ava` trên macOS và Linux hoặc `node_modules/.bin/ava.cmd` trên Windows. 

Trong `Application parameters` nhập các cờ CLI mà bạn đang sử dụng và các file test mà bạn muốn gỡ lỗi, ví dụ như `--verbose test.js`.

Trong `Node parameters`, cho phiên bản Node.js 7+, nhập vào cờ `--inspect-brk` để kích hoạt Node inspector. Đối với các phiên bản cũ hơn, sử dụng `--debug-brk`.

Lưu lại các cấu hình vừa thiết lập.

## Cài đặt dùng npm

Thực thi lệnh `npx @ava/init` trong thư mục dự án của bạn và thêm AVA vào file `package.json` của bạn.

File `package.json` của bạn sẽ trông như này:

```json
{
	"name": "awesome-package",
	"scripts": {
		"test": "ava"
	},
	"devDependencies": {
		"ava": "^0.20.0"
	}
}
```

Thêm mới một *npm Run/Debug configuration*: chọn `Edit Configurations...` từ danh sách kéo xuống ở phía trên bên phải, sau đó nhấn vào `+` và chọn *npm*.

Sử dụng các tham số cấu hình sau:

- `package.json`: Đường dẫn đến file `package.json` trong thư mục dự án của bạn
- `Command`: `test`

IDE của bạn sau đó sẽ thực thi lệnh `npm run test` và do đó gọi `node_modules/.bin/ava` và AVA-configuration bạn đã chỉ định trong package.json của mình.

Trong `Node parameters`, với phiên bản Node.js 7+ truyền vào `--inspect-brk` hoặc `--debug-brk` cho các phiên bản cũ hơn.

Đừng quên chọn một thông dịch viên Node.js.

Lưu lại các cấu hình.

## Gỡ lỗi

Đặt các breakpoint trong code của bạn.

Nhấn nút `Debug` màu xanh nằm bên cạnh danh sách cấu hình ở trên cùng bên phải. *Cửa sổ công cụ gỡ lỗi* sẽ xuất hiện. Khi breakpoint được kích hoạt, bạn có thể đánh giá các biến và từng bước chạy trong code của mình. Khi gỡ lỗi nhiều file test, bạn có thể chuyển đổi giữa các process bằng cách sử dụng menu thả xuống trong khung Frames.
