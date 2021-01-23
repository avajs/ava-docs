# JSPM và SystemJS cho ES2015

Translations: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/jspm-systemjs.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/recipes/jspm-systemjs.md)

Việc này đòi hỏi một trình trợ giúp loader đặc biệt để giải quyết chính xác các gói `import`s của các package trong JSPM khi bạn sử dụng AVA. Mục đích của loader là cho phép bạn chạy test của mình mà không cần phải xây dựng trước dự án JSPM của bạn.

## Cài đặt

Công thức này chỉ được test với JSPM v0.17.0-beta.22, nhưng nó sẽ hoạt động với bất kì phiên bản nào của JSPM v0.17 và có thể hoạt động với v0.16.

### Babel

Cài đặt .babelrc của bạn để làm việc với AVA nếu bạn chưa có. LƯU Ý: Bạn có thể giữ cấu hình bổ sung trong các tệp cấu hình JSPM của mình để ghi đè các cài đặt này trong quá trình đóng gói và xây dựng.

```
$ npm install --save-dev @babel/preset-env
```

```json
{
	"presets": ["@babel/preset-env"]
}
```

Bạn có thể tìm thêm thông tin về việc thiết lập Babel với AVA trong [Công thức Babel](babel.md).

### Trình trợ giúp JSPM Loader

Bạn sẽ cần phải cài đặt [AVA JSPM loader](https://github.com/skorlir/ava-jspm-loader) như là một dev dependency. Bạn sẽ cần phải cài đặt [`@babel/register`](https://www.npmjs.com/package/@babel/register).

```
$ npm install --save-dev ava-jspm-loader @babel/register
```
Bạn cũng sẽ cần cập nhật cấu hình AVA trong `package.json` hoặc `ava.config.js` để dùng JSPM loader.

```json
{
	"ava": {
		"require": [
			"@babel/register",
			"ava-jspm-loader"
		]
	}
}
```

NODE: Nếu bạn dùng async/await trong source của bạn (không phải trong code test), bạn sẽ cần phải cài [`@babel/polyfill`](https://www.npmjs.com/package/@babel/polyfill) và thêm nó vào mảng `require`.

### Ví dụ file test

Lưu ý rằng bạn sẽ cần sử dụng đường dẫn `System.import` cho tất cả các file dự án của bạn. Vì vậy, nếu bạn đã đặt tên cho dự án của mình là `app` và bạn muốn import `main.js` của mình vào một file test, bạn sẽ cần phải `import main từ 'app/main'`.

```js
import test from 'ava';
import main from 'app/main';  // Map tới cấu hình JSPM của bạn cho "app/main.js"
import BigNumber from 'bignumber.js';  // Trong jspm_packages

function fn() {
	return Promise.resolve(new BigNumber('1234567890.123456789'));
}

test('example test', async t => {
	t.is((await fn()).toString(), '1234567890.123456789');
});
```
