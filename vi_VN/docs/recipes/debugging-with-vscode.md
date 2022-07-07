# Gỡ lỗi cho các test với Visual Studio Code

Các bản dịch: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/debugging-with-vscode.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/recipes/debugging-with-vscode.md)

## Cài đặt

Trong thanh bên cạnh, nhấp vào `Debug`.

Thêm vào cấu hình mới trong trình đơn thả xuống bên cạnh nút `Debug` màu xanh: `Add configuration`. Thao tác này sẽ mở `launch.json` với tất cả cấu hình debug.

Thêm các cấu hình sau vào đối tượng `configurations`:

```json
{
	"type": "node",
	"request": "launch",
	"name": "Run AVA test",
	"program": "${workspaceRoot}/node_modules/ava/profile.js",
	"args": [
	  "${file}"
	],
	"skipFiles": [
		"<node_internals>/**/*.js"
	]
}
```

Lưu cấu hình này lại sau khi đã tinh chỉnh.

## Gỡ lỗi

> **Lưu ý:** File mà bạn muốn gỡ lỗi, phải đang mở và hoạt động

> **Lưu ý:** Các breakpoint trong VSCode thỉnh thoảng hơi bị lỗi (đặc biệt là với code không đồng bộ). `debugger;`, luôn hoạt động ổn.

Đặt các breakpoint vào code **hoặc** viết `debugger;` tại điểm cần dừng.

Nhấn nút `Debug` màu xanh bên cạnh danh sách các cấu hình ở trên cùng bên trái trong chế độ xem `Debug`. Khi breakpoint được nhấn, bạn có thể đánh giá các biến và từng bước mà code chạy.

## Gỡ lỗi nối tiếp

Theo mặc định, AVA chạy các test đồng thời. Điều này có thể làm thêm phức tạp cho việc gỡ lỗi. Thêm một cấu hình với đối số `--serial` để AVA chỉ chạy một test tại một thời điểm:

```json
{
	"type": "node",
	"request": "launch",
	"name": "Run AVA test serially",
	"program": "${workspaceRoot}/node_modules/ava/profile.js",
	"args": [
	  "--serial",
	  "${file}"
	],
	"skipFiles": [
		"<node_internals>/**/*.js"
	]
}
```

*Lưu ý rằng, nếu các test của bạn không được phân chia đúng cách, các lỗi kiểm tra nhất định có thể không xuất hiện khi chạy test một cách tuần tự.*
