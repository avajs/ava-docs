# Test các component của React

Các bản dịch: [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/docs/recipes/react.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/react.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/recipes/react.md)

## Cài đặt Babel

AVA tự động mở rộng cấu hình Babel của bạn (cấp dự án). Bạn sẽ có thể sử dụng React trong các file test của mình mà không cần bất kỳ cấu hình bổ sung nào khác.

Tuy nhiên, nếu bạn muốn thiết lập này rõ ràng, hãy thêm giá trị preset vào các tùy chọn kiểm tra trong Babel pipeline bằng cách sửa đổi file `package.json` hoặc `ava.config.js` của bạn:

```json
{
	"ava": {
		"babel": {
			"testOptions": {
				"presets": ["@babel/preset-react"]
			}
		}
	}
}
```

Bạn có thể tìm thêm thông tin về việc thiết lập Babel cho AVA trong [Công thức Babel](babel.md).

## Sử dụng [Enzyme](https://github.com/airbnb/enzyme)

Đầu tiên hãy xem cách sử dụng AVA với một trong các thư viện test React phổ biến nhất: [Enzyme](https://github.com/airbnb/enzyme).

Nếu bạn dự định chỉ sử dụng [shallow component rendering](https://facebook.github.io/react/docs/test-utils.html#shallow-rendering), bạn không cần bất kỳ thiết lập bổ sung nào khác.

Đầu tiên hãy cài đặt [Các package cần thiết của Enzyme](https://github.com/airbnb/enzyme/#installation):

```console
$ npm install --save-dev enzyme react-addons-test-utils react-dom
```

Sau đó bạn đã có thể sử dụng Enzyme ngay:

```js
import test from 'ava';
import React from 'react';
import {shallow} from 'enzyme';

const Foo = ({children}) =>
	<div className="Foo">
		<span className="bar">bar</span>
		{children}
		<span className="bar">bar</span>
	</div>;

Foo.propTypes = {
	children: React.PropTypes.any
};

test('has a .Foo class name', t => {
	const wrapper = shallow(<Foo/>);
	t.true(wrapper.hasClass('Foo'));
});

test('renders two `.Bar`', t => {
	const wrapper = shallow(<Foo/>);
	t.is(wrapper.find('.bar').length, 2);
});

test('renders children when passed in', t => {
	const wrapper = shallow(
		<Foo>
			<div className="unique"/>
		</Foo>
	);
	t.true(wrapper.contains(<div className="unique"/>));
});
```

Enzyme cũng có một helper `mount` và `render` để test trong môi trường browser thực tế. Nếu bạn muốn sử dụng những helper này, bạn sẽ cần phải thiết lập một môi trường browser. Xem [Công thức test browser](https://github.com/avajs/ava/blob/master/docs/recipes/browser-testing.md) để biết cách thực hiện.

Để xem ví dụ về AVA làm việc cùng với Enzyme được thiết lập cho việc test browser,   hãy xem [dự án mẫu này](https://github.com/adriantoine/ava-enzyme-demo).

Đây là một ví dụ cơ bản về cách tích hợp Enzyme với AVA. Để biết thêm thông tin về việc sử dụng Enzyme để thực hiện unit test cho React component, hãy xem [Tài liệu của Enzyme](http://airbnb.io/enzyme/).

## Sử dụng các helper của JSX

Một cách khác để test component của React đó là sử dụng package [`react-element-to-jsx-string`](https://github.com/algolia/react-element-to-jsx-string) để so sánh các cây DOM như là các chuỗi. [`jsx-test-helpers`](https://github.com/MoOx/jsx-test-helpers) là một thư viện tốt để [shallow component rendering](https://facebook.github.io/react/docs/test-utils.html#shallow-rendering) và chuyển đổi JSX thành chuỗi để kiểm tra các component của React bằng cách sử dụng các xác nhận giá trị của AVA.

```console
$ npm install --save-dev jsx-test-helpers
```

Cách sử dụng:

```js
import test from 'ava';
import React from 'react';
import {renderJSX, JSX} from 'jsx-test-helpers';

const Foo = ({children}) =>
	<div className="Foo">
		<span className="bar">bar</span>
		{children}
		<span className="bar">bar</span>
	</div>;

Foo.propTypes = {
	children: React.PropTypes.any
};

test('renders correct markup', t => {
	const actual = renderJSX(<Foo/>);
	const expected = JSX(
		<div className="Foo">
			<span className="bar">bar</span>
			<span className="bar">bar</span>
		</div>
	);
	t.is(actual, expected);
});

test('renders children when passed in', t => {
	const actual = renderJSX(
		<Foo>
			<div className="unique"/>
		</Foo>
	);
	const expected = JSX(
		<div className="Foo">
			<span className="bar">bar</span>
			<div className="unique"/>
			<span className="bar">bar</span>
		</div>
	);
	t.is(actual, expected);
});
```

Lưu ý rằng bạn phải sử dụng các biến như `actual` và `expected` vì [`power-assert` không xử lý JSX một cách chính xác](https://github.com/power-assert-js/power-assert/issues/34).

Đây là một ví dự cơ bản về cách sử dụng `jsx-test-helpers` với AVA. Để xem cách sử dụng nâng cao của thư viện này, hãy xem [ghi chú của file test này](https://github.com/MoOx/jsx-test-helpers/blob/master/src/__tests__/index.js).

[Dự án mẫu này](https://github.com/MoOx/jsx-test-helpers) hiển thị sự thiết lập cơ bản và tối thiểu của AVA với `jsx-test-helpers`.

## Sử dụng các thư việc xác nhận giá trị khác

Trong AVA, bạn có thể sử dụng bất kỳ thư viện xác nhận giá trị nào, và hiện nay đã có một số thư viện để test component của React. Dưới đây là danh sách các thư viện hoạt động tốt với AVA:

- [`expect-jsx`](https://github.com/algolia/expect-jsx) ([Ví dụ](https://github.com/avajs/ava/issues/186#issuecomment-161317068))
- [`unexpected-react`](https://github.com/bruderstein/unexpected-react) ([Dự án mẫu với ví dụ output](https://github.com/adriantoine/ava-unexpected-react-demo))
