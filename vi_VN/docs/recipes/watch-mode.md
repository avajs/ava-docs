# Chế độ theo dõi

Các bản dịch: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/watch-mode.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/watch-mode.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/watch-mode.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/watch-mode.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/recipes/watch-mode.md)

AVA đi kèm với chế độ theo dõi thông minh. Nó sẽ theo dõi sự thay đổi của các file và chỉ chạy test của các file có sự ảnh hưởng.

## Chạy test với chế độ theo dõi được bật

Bạn có thể bật chế độ theo dõi bằng cách sử dụng các cờ `--watch` hoặc `-w`. Nếu bạn đã cài đặt AVA toàn cục:

```console
$ ava --watch
```

Nếu bạn đã cấu hình nó trong `package.json` như thế này:

```json
{
	"scripts": {
		"test": "ava"
	}
}
```

Bạn có thể chạy:

```console
$ npm test -- --watch
```

Bạn cũng có thể thiết lập một lệnh đặc biệt:

```json
{
	"scripts": {
		"test": "ava",
		"watch:test": "ava --watch"
	}
}
```

Và sau đó dùng:

```console
$ npm run watch:test
```

Cuối cùng, bạn có thể định cấu hình AVA thành *luôn* chạy trong chế độ theo dõi bằng cách đặt khóa `watch` trong [`ava` của file `package.json`, hoặc `ava.config.js`][cấu hình]:

```json
{
	"ava": {
		"watch": true
	}
}
```

Hãy lưu ý rằng TAP reporter không khả dụng khi sử dụng chế độ theo dõi.

## Các yêu cầu

AVA sử dụng [`chokidar`] làm trình theo dõi file. Lưu ý rằng ngay cả khi bạn thấy cảnh báo về các dependency bị lỗi cài đặt, nó vẫn sẽ hoạt động tốt. Vui lòng tham khảo phần *[Install Troubleshooting]* của tài liệu `chokidar` để biết cách giải quyết các vấn đề cài đặt với chokidar.

## Các file source và file test

Trong AVA, có sự phân biệt giữa *file source* và *file test*. Bạn có thể tưởng tượng *các file test* chứa các test kiểm tra của bạn. *Các file source* là tất cả các file cần thiết để cho các test có thể chạy, có thể là mã nguồn của bạn hoặc các thử nghiệm test.

Theo mặc định, AVA theo dõi các thay đổi đối với các file test, các file snapshot, `package.json`, và các file `.js` khác. Nó sẽ bỏ qua các file trong [các thư mục nhất định](https://github.com/novemberborn/ignore-by-default/blob/master/index.js) như đã được cung cấp bởi package [`ignore-by-default`].

Bạn có thể cấu hình các pattern cho các file source trong [phần `ava` của file `package.json`, hoặc `ava.config.js`][config], sử dụng khóa `sources`.

Bạn có thể chỉ định các pattern để khớp với các file trong các thư mục mà nếu không chúng sẽ bị bỏ qua, ví dụ: sử dụng `node_modules/some-dependency/*.js` để chỉ định tất cả các tệp `.js` trong `node_modules/some-dependency` như là nguồn, mặc dù thông thường tất cả các tệp trong `node_modules` đều bị bỏ qua. Lưu ý rằng bạn cần chỉ định một thư mục chính xác; `{bower_components,node_modules}/**/*.js` sẽ không hoặt động.

Nếu các test của bạn ghi vào đĩa, chúng có thể kích hoạt trình giám sát chạy lại các thử nghiệm của bạn. Định cấu hình các pattern cho các file source để tránh điều này.

## Theo dõi dependency

AVA theo dõi các file source mà các file test của bạn phụ thuộc vào. Nếu bạn thay đổi một dependency thì chỉ có file test phụ thuộc vào nó sẽ chạy lại. AVA sẽ chạy lại tất cả các test nếu nó không thể xác định được file test nào phụ thuộc vào file source đã thay đổi.

Theo dõi sự phụ thuộc của dependency hoạt động cho các module bắt buộc. Các phần mở rộng tùy chỉnh và các transpiler được hỗ trợ, miễn là bạn [đã thêm chúng vào file `package.json` hoặc `ava.config.js`][config], chứ không phải từ bên trong file test của bạn. Các file được truy cập bằng module `fs` không được theo dõi.

## Chế độ theo dõi và modifier `.only`

[Modifier `.only`] vô hiệu hóa thuật toán theo dõi denpendency của chế độ theo dõi. Khi thay đổi được thực hiện, tất cả các test `.only` sẽ chạy lại, bất kể test có phụ thuộc vào file đã thay đổi hay không. 

## Chế độ theo dõi và CI

Nếu bạn chạy AVA trong CI với chế độ theo dõi, việc thực hiện sẽ thoát với một lỗi (`Lỗi: Chế độ theo dõi không khả dụng trong CI, vì nó ngăn AVA không thể kết thúc.`). AVA sẽ không chạy với tùy chọn `--watch` (`-w`) trong CI, bởi vì các quá trình CI sẽ kết thúc, và với tùy chọn `--watch`, AVA sẽ không bao giờ kết thúc được.

## Trả về tất cả test bằng cách thủ công

Bạn có thể nhanh chóng chạy lại tất cả các test bằng cách gõ <kbd>r</kbd> trên console, theo sau là <kbd>Enter</kbd>.

## Cập nhật các snapshot

Bạn có thể cập nhật các snapshot không thành công bằng cách gõ <kbd>u</kbd> trên console, theo sau là <kbd>Enter</kbd>.

## Gỡ lỗi

Đôi khi chế độ theo dõi có thể làm vài điều ngạc nhiên như chạy lại tất cả các test khi bạn nghĩ chỉ có một test duy nhất sẽ được chạy. Để xem lý do, bạn có thể bật chế độ gỡ lỗi. Điều này sẽ làm việc tốt nhất với các báo cáo chi tiết:

```console
$ DEBUG=ava:watcher npm test -- --watch --verbose
```

Trên Windows dùng:

```console
$ set DEBUG=ava:watcher
$ npm test -- --watch --verbose
```

## Giúp chúng tôi cải thiện chế độ theo dõi

Chế độ theo dõi là tương đối mới và có thể có một số lỗi. Hãy [báo cáo](https://github.com/avajs/ava/issues) bất kì vấn đề nào bạn gặp phải. Xin cảm ơn!

[`chokidar`]: https://github.com/paulmillr/chokidar
[Install Troubleshooting]: https://github.com/paulmillr/chokidar#install-troubleshooting
[`ignore-by-default`]: https://github.com/novemberborn/ignore-by-default
[`.only` modifier]: https://github.com/avajs/ava#running-specific-tests
[config]: https://github.com/avajs/ava#configuration
