# Code coverage

Các bản dịch: [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/docs/recipes/code-coverage.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/code-coverage.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/code-coverage.md), [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/docs/recipes/code-coverage.md), [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/docs/recipes/code-coverage.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/code-coverage.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/code-coverage.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/recipes/code-coverage.md)

Sử dụng [nyc] command-line-client cho [istanbul] để tính toán code coverage cho  các test của bạn.

Đầu tiên hãy cài đặt [nyc]:

```
$ npm install --save-dev nyc
```

Để đơn giản, hãy chạy AVA thông qua [nyc]. Trong file `package.json` của bạn:

```json
{
	"scripts": {
		"test": "nyc ava"
	}
}
```

Có thể bạn sẽ muốn loại trừ các thư mục `.nyc_output` và `coverage` khỏi source control. Giả sử bạn đang sử dụng Git, thêm phần sau đây vào file `.gitignore`:

```
.nyc_output
coverage
```

Nếu bạn đang biên dịch các file source của bạn bằng cách sử dụng Babel, bạn có thể sẽ muốn áp dụng thiết bị đo đạc của Istanbul như là một phần của quá trình biên dịch các file source. Điều này sẽ mang lại kết quả tốt hơn so với công cụ của Babel. Xem [*Sử dụng Istalbul với ES2015+* hướng dẫn](https://istanbul.js.org/docs/tutorials/es2015/). AVA sẽ đặt `NODE_ENV=test` cho bạn. Lưu ý rằng kể từ tháng 2 năm 2018, hướng dẫn này chưa được cập nhật cho Babel 7.

[Istanbul]: https://istanbul.js.org/
[nyc]: https://github.com/istanbuljs/nyc
