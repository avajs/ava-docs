# Truyền tham số vào các file test

Các bản dịch: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/passing-arguments-to-your-test-files.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/recipes/passing-arguments-to-your-test-files.md)

Bạn có thể truyền tham số dòng lệnh cho các file test của mình. Sử dụng đối số dòng lệnh `--` để tách các đối số của AVA khỏi chính đối số của bạn:

```js
// test.js
import test from 'ava';

test('argv', t => {
	t.deepEqual(process.argv.slice(2), ['--hello', 'world']);
});
```

```console
$ npx ava -- --hello world
```

Bạn cần hai đối số dòng lệnh `--` nếu bạn gọi AVA thông qua một lệnh `npm test`:

```json
{
	"scripts": {
		"test": "ava"
	}
}
```

```console
$ npm test -- -- --hello world
```
