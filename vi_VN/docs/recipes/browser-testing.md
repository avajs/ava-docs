# Thiết lập AVA để test browser

Các bản dịch: [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/docs/recipes/browser-testing.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/browser-testing.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/browser-testing.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/browser-testing.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/browser-testing.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/recipes/browser-testing.md)

AVA [vẫn chưa](https://github.com/avajs/ava/issues/24) hỗ trợ chạy test trên browser. Tuy nhiên, các thư viện JavaScript yêu cầu các global cụ thể của browser (`window`, `document`, `navigator`, v.v.) vẫn có thể được kiểm tra bằng AVA, bằng cách mô phỏng các global này.

Công thức này hoạt động cho bất kỳ thư viện nào cần một môi trường mô phỏng browser.

## Cài đặt browser-env

> **❗️ Lưu ý quan trọng**
>
>`browser-env` thêm vào các thuộc tính từ window namespace `jsdom` vào namespace toàn cục của Node.js. Điều này rõ ràng là [không được khuyến khích](https://github.com/tmpvar/jsdom/wiki/Don't-stuff-jsdom-globals-onto-the-Node-global) bởi `jsdom`. Vui lòng đọc qua trang wiki được liên kết và đảm bảo rằng bạn hiểu được các sự phản đối. Nếu bạn không có nhiều dependency cũng yêu cầu một môi trường browser thì [`window`](https://github.com/lukechilds/window#universal-testing-pattern) có thể là một giải pháp tốt hơn.

Cài đặt [browser-env](https://github.com/lukechilds/browser-env).

> Mô phỏng môi trường browser toàn cục bằng jsdom.

```
$ npm install --save-dev browser-env
```

## Cài đặt browser-env

Tạo một file helper và đặt nó bên trong thư mục `test/helpers`. Điều này để đảm bảo AVA không coi nó là một file test.

`test/helpers/setup-browser-env.js`:

```js
import browserEnv from 'browser-env';
browserEnv();
```

Theo mặc định, `browser-env` sẽ thêm tất cả các biến của browser vào phạm vi toàn cục của Node.js, tạo ra một môi trường browser hoàn chỉnh. Điều này có khả năng tương thích tốt với hầu hết các thư viện front-end, tuy nhiên, nói chung thì nó cũng không phải là một ý tường hay khi tạo nhiều biến toàn cục nếu bạn không cần đến chúng. Nếu bạn biết chính xác browser nào bạn cần, bạn có thể truyền vào một mảng của chúng.

```js
import browserEnv from 'browser-env';
browserEnv(['window', 'document', 'navigator']);
```

Bạn có thể phơi bày ra nhiều biến toàn cục hơn bằng cách gán chúng cho đối tượng `global`. Ví dụ, jQuery thường có sẵn thông qua biến `$`:

```js
import browserEnv from 'browser-env';
import jQuery from 'jquery';

browserEnv();
global.$ = jQuery(window);
```

## Cấu hình test đề dùng browser-env

Cấu hình để AVA `require` helper trước mỗi file test.

`package.json`:

```json
{
	"ava": {
		"require": [
			"./test/helpers/setup-browser-env.js"
		]
	}
}
```

## Thưởng thức thôi!

Viết các test của bạn và tận hưởng một môi trường mô phỏng browser.

`test.js`:

```js
import test from 'ava';

test('Insert to DOM', t => {
	const div = document.createElement('div');
	document.body.appendChild(div);

	t.is(document.querySelector('div'), div);
});
```
