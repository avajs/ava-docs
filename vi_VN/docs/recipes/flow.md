# Flow

Các bản dịch: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/flow.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/recipes/flow.md)

AVA đi kèm với một file định nghĩa Flow. Điều này cho phép các nhà phát triển tận dụng Flow để viết test.

Hướng dẫn này giả định là bạn đã thiết lập Flow cho dự án của mình. Lưu ý rằng định nghĩa của AVA đã được kiểm tra với phiên bản 0.73.0.

Chúng tôi khuyên bạn nên sử dụng Babel pipeline tích hợp sẵn của AVA để loại bỏ annotations và declarations của Flow. AVA tự động áp dụng cấu hình Babel trong dự án của bạn, vì vậy mọi thứ chỉ có thể hoạt động mà không có sự thay đổi. Ngoài ra, hãy cài đặt  [`@babel/plugin-transform-flow-strip-types`](https://www.npmjs.com/package/@babel/plugin-transform-flow-strip-types) và tùy chỉnh cấu hình của AVA trong file `package.json` (hoặc file `ava.config.js`) như sau:

```json
{
	"ava": {
		"babel": {
			"testOptions": {
				"plugins": ["@babel/plugin-transform-flow-strip-types"]
			}
		}
	}
}
```

Xem [Tài liệu Babel](babel.md) của chúng tôi để biết thêm chi tiết.

## Viết tests

Tạo một file `test.js`.

```js
// @flow
import test from 'ava';

const fn = async () => Promise.resolve('foo');

test(async (t) => {
	t.is(await fn(), 'foo');
});
```

## Typing [`t.context`](https://github.com/avajs/ava#test-context)

Theo mặc định, kiểu `t.context` sẽ là đối tượng rỗng (`{}`). AVA chìa ra một interface `TestInterface<Context>` mà bạn có thể sử dụng để áp dụng kiểu của riêng bạn cho `t.context`. Điều này có thể giúp bạn nắm bắt được lỗi tại thời điểm biên dịch:

```js
// @flow
import anyTest from 'ava';
import type {TestInterface} from 'ava';

const test: TestInterface<{foo: string}> = (anyTest: any);

test.beforeEach(t => {
	t.context = {foo: 'bar'};
});

test.beforeEach(t => {
	t.context.foo = 123; // lỗi: Kiểu '123' không thể gán cho kiểu 'string'
});

test.serial.cb.failing('very long chains are properly typed', t => {
	t.context.fooo = 'a value'; // lỗi: Thuộc tính 'fooo' không tồn tại trong loại ''
});

test('an actual test', t => {
	t.deepEqual(t.context.foo.map(c => c), ['b', 'a', 'r']); // lỗi: Thuộc tính 'map' không tồn tại trong loại 'string'
});
```

Lưu ý rằng, mặc dù kiểu được truyền ở trên, khi thực thi, `t.context` là một đối tượng trừ khi nó được gán.
