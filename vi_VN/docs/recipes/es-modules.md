# Sử dụng các module ES trong AVA

Các bản dịch: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/es-modules.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/recipes/es-modules.md)

Kể từ phiên bản Node.js 8.5.0, [ES modules](http://2ality.com/2017/09/native-esm-node.html) được hỗ trợ sẵn, nhưng phải dùng với cờ  `--experimental-modules`. Nó hoạt động bằng cách sử dụng phần mở rộng của tập tin `.mjs`. AVA hiện không hỗ trợ tùy chọn command line hoặc đuôi file mở rộng mới, nhưng bạn *có thể* sử dụng module [`esm`](https://github.com/standard-things/esm) để sử dụng các cú pháp mới.

Đây là cách bạn làm cho nó hoạt động với AVA.

Đầu tiên, cài đặt [`esm`](https://github.com/standard-things/esm):

```
$ npm install esm
```

Cấu hình nó trong tệp `package.json` hoặc `ava.config.js` của bạn và thêm nó vào tùy chọn `"require"` của AVA. Đảm bảo thêm nó vào mục đầu tiên:

```json
{
	"ava": {
		"require": [
			"esm"
		]
	}
}
```

Theo mặc định, AVA sẽ chuyển đối cú pháp module ES sang CommonJS. [Bạn có thể vô hiệu hóa điều này](./babel.md#preserve-es-module-syntax).

Bây giờ bạn có thể sử dụng các module ES được hỗ trợ với AVA:

```js
// sum.mjs
export default function sum(a, b) {
	return a + b;
};
```

```js
// test.js
import test from 'ava';
import sum from './sum.mjs';

test('2 + 2 = 4', t => {
	t.is(sum(2, 2), 4);
});
```

Bạn cần phải cấu hình AVA để nó nhận ra các đuôi mở rộng `.mjs`. Nếu bạn muốn AVA áp dụng Babel preset của nó, hãy sử dụng:

```json
{
	"ava": {
		"babel": {
			"extensions": [
				"js",
				"mjs"
			]
		}
	}
}
```

Hoặc bạn có thể sử dụng:

```json
{
	"ava": {
		"babel": false,
		"extensions": [
			"js",
			"mjs"
		]
	}
}
```

Hoặc để cho Babel được kích hoạt (có nghĩa là nó được áp dụng cho các tệp `.js`), nhưng không được áp dụng cho các file `.mjs`:

```json
{
	"ava": {
		"extensions": [
			"mjs"
		]
	}
}
```
