# Test các component của Vue.js

Các bản dịch: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/vue.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/recipes/vue.md)

## Các dependency

- [Cần các hook mở rộng](https://github.com/jackmellis/require-extension-hooks):
	- `npm i --save-dev require-extension-hooks require-extension-hooks-vue require-extension-hooks-babel`

- [browser-env](browser-testing.md)
	- `npm i --save-dev browser-env`

## Cài đặt

Bước đầu tiên là bạn phải thiết lập một helper giúp cấu hình môi trường để transpile các file `.vue` và chạy trong một môi trường như browser:

`package.json`

```json
{
	"ava": {
		"require": [
			"./test/helpers/setup.js"
		]
	}
}
```

```js
// ./test/helpers/setup.js

// Cài đặt môi trường browser
require('browser-env')();
const hooks = require('require-extension-hooks');
const Vue = require('vue');

// Mẹo thiết lập Vue.js để loại bỏ production
Vue.config.productionTip = false;

// Thiết lập các file vue được xử lý bởi`require-extension-hooks-vue`
hooks('vue').plugin('vue').push();
// Cài đặt các file vue và js để xử lý bởi `require-extension-hooks-babel`
hooks(['vue', 'js']).plugin('babel').push();
```

Bạn có thể tìm thêm nhiều thông tin về việc thiết lập Babel với AVA trong [Công thức Babel](babel.md).

## Test snapshot đơn giản

```js
import test from 'ava';
import Vue from 'vue';
import Component from 'component.vue';

test('renders', t => {
	const vm = new Vue(Component).$mount();
	const tree = {
		$el: vm.$el.outerHTML
	};
	t.snapshot(tree);
});
```

## Báo cáo Coverage

Thực hiện theo [công thức báo cáo coverage](code-coverage.md), bổ sung thêm phần mở rộng `.vue` vào cấu hình `nyc` của tập tin `.vue`.

```json
{
	"nyc": {
		"extension": [
			".js",
			".vue"
		]
	}
}
```
