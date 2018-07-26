___
**Lưu ý của người phiên dịch**

Đây là bản dịch của file [readme.md](https://github.com/avajs/ava/blob/master/readme.md). Đây là [liên kết]() để thấy sự khác biệt với nhánh chính của AVA (Nếu bạn nhấp vào liên kết mà không thấy có sự thay đổi đối với file `readme.md` thì có nghĩa là bản dịch đã được cập nhật).
___
# [![AVA](media/header.png)](https://ava.li)

> Test runner của tương lai 

[![Build Status: Linux](https://travis-ci.org/avajs/ava.svg?branch=master)](https://travis-ci.org/avajs/ava) [![Build status: Windows](https://ci.appveyor.com/api/projects/status/e7v91mu2m5x48ehx/branch/master?svg=true)](https://ci.appveyor.com/project/ava/ava/branch/master) [![Coverage Status](https://coveralls.io/repos/github/avajs/ava/badge.svg?branch=master)](https://coveralls.io/github/avajs/ava?branch=master) [![XO code style](https://img.shields.io/badge/code_style-XO-5ed9c7.svg)](https://github.com/xojs/xo) [![Join the community on Spectrum](https://withspectrum.github.io/badge/badge.svg)](https://spectrum.chat/ava)
 [![Mentioned in Awesome Node.js](https://awesome.re/mentioned-badge.svg)](https://github.com/sindresorhus/awesome-nodejs)

Mặc dù JavaScript là một ngôn ngữ đơn luồng, nhưng IO trong Node.js có thể thực thi song song do tính chất không đồng bộ của nó. AVA tận dụng lợi thế này để chạy các test của bạn một cách đồng thời, điều này đặc biệt có lợi cho việc thực thi các test IO cồng kềnh. Ngoài ra, các file test được chạy song song như các quy trình riêng biệt, cho bạn hiệu năng tốt hơn và một môi trường độc lập cho mỗi file test. [Chuyển](https://github.com/sindresorhus/pageres/commit/663be15acb3dd2eb0f71b1956ef28c2cd3fdeed0) từ Mocha sang AVA trong dự án Pageres đã giảm thời gian chạy test từ 31 xuống còn 11 giây. Việc có các test chạy đồng thời buộc bạn phải viết từng test nhỏ, chi tiết, có ý nghĩa và không phụ thuộc vào trạng thái toàn cục hoặc trạng thái của các test khác, đó là một điều tuyệt vời!

![](https://github.com/avajs/ava/blob/master/media/mini-reporter.gif)

*Đọc [hướng dẫn đóng góp](contributing.md) nếu bạn muốn đóng góp (issues/PRs/.v.v).*

Theo dõi [Tài khoản Twitter của AVA](https://twitter.com/ava__js) để biết các cập nhật mới.

**Tài liệu này thể theo phiên bản 1.0 beta, sử dụng Babel 7. Phiên bản mới nhất sử dụng Babel 6 là [`v0.25.0`](https://github.com/avajs/ava/tree/v0.25.0).**

Các bản dịch [Español](https://github.com/avajs/ava-docs/blob/master/es_ES/readme.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/readme.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/readme.md), [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/readme.md), [한국어](https://github.com/avajs/ava-docs/blob/master/ko_KR/readme.md), [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/readme.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/readme.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/readme.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/readme.md)


## Nội dung

- [Cách sử dụng](#cách-sử-dụng)
- [Cách sử dụng CLI](#cli)
- [Kiểm lỗi](#kiểm-lỗi)
- [Báo cáo](#báo-cáo)
- [Cấu hình](#cấu-hình)
- [Tài liệu](#tài-liệu)
- [API](#api)
- [Xác nhận giá trị](#xác-nhận-giá-trị)
- [Test với snapshot](#test-với-snapshot)
- [Các mẹo](#các-mẹo)
- [FAQ](#faq)
- [Các công thức](#các-công-thức)
- [Hỗ trợ](#hỗ-trợ)
- [Có liên quan](#có-liên-quan)
- [Các liên kết](#các-liên-kết)
- [Nhóm phát triển](#nhóm-phát-triển)


## Tại sao dùng AVA?

- Nhỏ gọn và nhanh chóng
- Cú pháp để test đơn giản
- Chạy test đồng thời
- Ép buộc viết test chi tiết
- Không có giá trị toàn cục hiện hữu 
- Bao gồm các định nghĩa kiểu TypeScript & Flow
- [Cú pháp test thần kì](#cú-pháp-test-thần-kì)
- [Môi trường riêng biệt cho mỗi file test](#môi-trường-riêng-biệt-cho-mỗi-file-test)
- [Viết test sử dụng cú pháp Javascript mới nhất](#latest-javascript-support)
- [Hỗ trợ Promise](#promise-support)
- [Hỗ trợ hàm bất đồng bộ](#async-function-support)
- [Hỗ trợ Observable](#observable-support)
- [Cải thiện thông báo test](#enhanced-assertion-messages)
- [Test chạy song song và tự động trên CI](#parallel-runs-in-ci)
- [TAP reporter](#tap-reporter)
- [Tự động chuyển đổi từ các test runner khác](https://github.com/avajs/ava-codemods#migrating-to-ava)


## Cú pháp test

```js
import test from 'ava';

test('arrays are equal', t => {
	t.deepEqual([1, 2], [1, 2]);
});
```

## Cách sử dụng

### Thêm AVA vào dự án của bạn

Để cài đặt AVA, thực thi lệnh:

```console
$ npx create-ava --next
```

File `package.json` sau đó sẽ như thế này:

```json
{
	"name": "awesome-package",
	"scripts": {
		"test": "ava"
	},
	"devDependencies": {
		"ava": "1.0.0-beta.4"
	}
}
```

Việc khởi tạo sẽ hoạt động với npm và Yarn, nhưng để chạy 'npx' bạn cần có  [`npm@5.2.0`](https://github.com/npm/npm/releases/tag/v5.2.0) hoặc phiên bản mới hơn. Nếu không, bạn sẽ phải tự cài đặt `ava` và cấu hình `test` script trong `package.json` như bên trên:

```console
$ npm install --save-dev --save-exact ava@next
```

Hoặc nếu bạn thích dùng Yarn:

```console
$ yarn add ava@next --dev --exact
```

### Tạo file test của bạn

Tạo một file có tên `test.js` trong thư mục gốc của dự án với nội dung như sau:

```js
import test from 'ava';

test('foo', t => {
	t.pass();
});

test('bar', async t => {
	const bar = Promise.resolve('bar');

	t.is(await bar, 'bar');
});
```

### Chạy test

```console
$ npm test
```

### Theo dõi test

```console
$ npm test -- --watch
```

AVA đi kèm với chế độ theo dõi thông minh. [Xem thêm tại công thức](docs/recipes/watch-mode.md).

### Các phiên bản Node.js được hỗ trợ

AVA hỗ trợ các bản release mới nhất của bất kì phiên bản chính nào [được hỗ trợ bởi chính Node.js](https://github.com/nodejs/Release#release-schedule). Đọc thêm tại [các sự hỗ trợ](docs/support-statement.md) của chúng tôi.

## CLI

```console
$ ava --help

  Cách dùng
    ava [<file|directory|glob> ...]

  Các tùy chọn
    --watch, -w             Chạy lại test khi test và file test có sự thay đổi
    --match, -m             Chỉ chạy tets với các tiêu đề phù hợp (Có thể lặp lại)
    --update-snapshots, -u  Cập nhật snapshots
    --fail-fast             Dừng ngay khi test đầu tiên thất bại
    --timeout, -T           Thiết lập thời gian timeout toàn cục
    --serial, -s            Chạy các test theo từng kỳ
    --concurrency, -c       Số test tối đa cùng chạy ở một thời điểm (Mặc định: Số nhân của CPU)
    --verbose, -v           Hiển thị chi tiết các output
    --tap, -t               Tạo TAP output
    --color                 Màu của output
    --no-color              Vô hiệu hóa màu cho output

  Ví dụ
    ava
    ava test.js test2.js
    ava test-*.js
    ava test

  Khuôn mẫu mặc định khi không có tham số:
  test.js test-*.js test/**/*.js **/__tests__/**/*.js **/*.test.js
```

*Lưu ý rằng CLI sẽ sử dụng cài đặt cục bộ của AVA khi có thể, ngay cả khi bạn chạy lệnh toàn cục.*

Các thư mục được đệ quy, tất cả các file có đuôi `*.js` sẽ được xem như là file test. Các thư mục có tên `fixtures`, `helpers` và `node_modules` sẽ *luôn* bị bỏ qua. Các file có tên bắt đầu bằng `_` sẽ cho phép bạn đặt những code hữu ích trong cùng thư mục với file test của bạn.

Khi sử dụng lệnh `npm test`, bạn có thể truyền vào trực tiếp các tham số để chỉ rõ file cần test `npm test test2.js`, nhưng các tham số nên được truyền vào như thế như sau `npm test -- --verbose`.


## Kiểm lỗi

AVA chạy các test trong các process con, do đó, để kiểm lỗi, bạn cần thực hiện giải pháp sau:

```console
$ node --inspect node_modules/ava/profile.js some/test/file.js
```

### Các mẹo riêng để gỡ lỗi

- [Chrome DevTools](docs/recipes/debugging-with-chrome-devtools.md)
- [WebStorm](docs/recipes/debugging-with-webstorm.md)
- [Visual Studio Code](docs/recipes/debugging-with-vscode.md)


## Báo cáo

### Báo cáo mini

Báo cáo mini là báo cáo mặc định.

<img src="media/mini-reporter.gif" width="460">

### Báo cáo chi tiết

Dùng tham số `--verbose` để kích hoạt báo cáo chi tiết. Báo cáo chi tiết sẽ luôn được dùng trong môi trường CI, trừ khi [Báo cáo TAP](#tap-reporter) đã được kích hoạt.

<img src="media/verbose-reporter.png" width="294">

### Báo cáo TAP

AVA hỗ trợ định dạng TAP và do đó tương thích với [Bất kì báo cáo TAP nào khác](https://github.com/sindresorhus/awesome-tap#reporters). Sử dụng tham số `--tap` để bật báo cáo TAP.

```console
$ ava --tap | tap-nyan
```

<img src="media/tap-reporter.png" width="420">

Lưu ý rằng báo cáo TAP sẽ không khả dụng khi sử dụng [Theo dõi test](#theo-dõi-test).

### Cú pháp test thần kì

AVA thêm vào các trích đoạn code và các sự khác biệt rõ ràng cho từng giá trị thực tế và giá trị mong muốn. Nếu giá trị của cú pháp test có dạng đối tượng hoặc chuỗi, sẽ chỉ có một sự khác biệt được hiển thị, để loại bỏ đi các khác biệt nhiễu và tập trung vào vấn đề. Các sự khác biệt này cũng được làm nổi bật cú pháp! Nếu bạn đang so sánh chuỗi, cả trên 1 dòng và nhiều dòng, AVA sẽ hiển thị một loại output khác, AVA sẽ làm nổi bật các kí tự được thêm vào hoặc bị loại bỏ đi.

![](media/magic-assert-combined.png)

### Dọn dẹp stack traces

AVA sẽ tự động xóa đi các dòng không liên quan trong stack traces, cho phép bạn tìm ra nguồn gốc của lỗi nhanh hơn rất nhiều, như đã thấy ở trên.


## Cấu hình

Tất cả các tùy chọn của CLI đều có thể được cấu hình trong phần `ava` của file `package.json` hoặc `ava.config.js`. Điều này cho phép bạn thay đổi hành vi mặc định của lệnh `ava`, do đó bạn không phải gõ lặp đi lặp lại nhiều lần các tùy chọn.

Để bỏ qua một file hoặc một thư mục, hãy thêm tiền tố vào pattern bằng dấu `!` (Dấu chấm than).

```json
{
	"ava": {
		"files": [
			"my-test-directory/**/*.js",
			"!my-test-directory/exclude-this-directory/**/*.js",
			"!**/exclude-this-file.js"
		],
		"sources": [
			"**/*.{js,jsx}",
			"!dist/**/*"
		],
		"match": [
			"*oo",
			"!foo"
		],
		"cache": true,
		"concurrency": 5,
		"failFast": true,
		"failWithoutAssertions": false,
		"tap": true,
		"compileEnhancements": false,
		"require": [
			"@babel/register"
		],
		"babel": {
			"extensions": ["jsx"],
			"testOptions": {
				"babelrc": false
			}
		}
	}
}
```

Các tham số được truyền vào CLI sẽ luôn được ưu tiên hơn các tham số đã cấu hình trong file `package.json`.

### Các tùy chọn

- `files`: đường dẫn của file, thư mục và các pattern toàn cục nhằm xác định các file test mà AVA sẽ thực thi. Files với tiền tố là dấu gạch dưới sẽ bị bỏ qua. Tất cả các file trong thư mục đã được chọn đều sẽ được thực thi. Mặc định, AVA chỉ chọn các file có đuôi mở rộng là `js`, ngay cả khi pattern toàn cục khớp với các file khác. Chỉ định `extensions` và `babel.extensions` để cho phép file với đuôi mở rộng khác.
- `source`: các tệp mà khi có sự thay đổi, sẽ làm cho các test chạy lại trong chế độ theo dõi. Xem [các công thức để biết thêm chi tiết](https://github.com/avajs/ava/blob/master/docs/recipes/watch-mode.md#source-files-and-test-files)
- `match`: không hữu ích lắm trong việc cấu hình file  `package.json`, nhưng tương đương với [chỉ định `--mactch` trong CLI](#thực-thi-test-với-các-tiêu-đề-trùng-khớp)
- `cache`: cache biên dịch các file test và file hỗ trợ bằng `node_modules/.cache/ava`. Nếu giá trị là `false`, các file sẽ được lưu trong thư mục tạm thời
- `failFast`: ngừng thực thi test khi có một test chạy không thành công
- `failWithoutAssertions`: nếu giá trị là `false`, không tính một file test là thất bại nếu như nó chưa được chạy [Xác nhận giá trị](#xác-nhận-giá-trị)
- `tap`: nếu giá trị là `true`, kích hoạt [Báo cáo TAP](#báo-cáo-tap)
- `snapshotDir`: chỉ đị vị trí cố định để lưu các file snapshot. Sử dụng chức năng này nếu các file snapshot của bạn nằm sai vị trí
- `compileEnhancements`: nếu giá trị là `false`, vô hiệu hóa [power-assert](https://github.com/power-assert-js/power-assert) — giúp cung cấp thêm các thông báo mô tả lỗi — và phát hiện việc sử dụng `t.throws()` không đúng cách
- `extensions`: phần mở rộng của các file test không được biên dịch trước sử dụng các thiết lập sẵn của Babel trong AVA. Lưu ý rằng các file vẫn được biên dịch để kích hoạt power-assert và cách tính năng khác, vì vậy bạn có thể cần phải thiết lập `compileEnhancements` thành `false` nếu các file của bạn không đúng định dạng Javascript. Thiết lập này sẽ được ghi đè lên giá trị `"js"` mặc định, vì vậy hãy đảm bảo rằng bạn đã thêm phần mở rộng đó vào danh sách, miễn là nó không nằm trong `babel.extensions`
- `require`: các module bổ sung cần thiết trước khi chạy test. Các module cần thiế trong [worker processes](#process-isolation)
- `babel`: các tùy chọn cài đặt Babel cụ thể của file test. Xem [Công thức Babel] của chúng tôi để có thêm chi tiết
- `babel.extensions`: các phần mở rộng của các file test sẽ được biên dịch trước bằng các thiết lập Babel của AVA. Giá trị này sẽ được ghi đè lên giá trị `"js"` mặc định, vì vậy hãy đảm bảo rằng bạn đã thêm phần mở rộng đó trong danh sách

Lưu ý rằng việc cung cấp các file trên CLI sẽ ghi đè lên tùy chọn `files`. Nếu bạn đã cấu hình một pattern toàn cục, ví dụ `test/**/*.test.js`, bạn có lẽ sẽ muốn lặp lại nó khi sử dụng CLI: `ava 'test/integration/*.test.js'`.

### Sử dụng `ava.config.js`

Để sử dụng file cấu hình `ava.config.js`:

 1. Nó phải nằm trong cùng thư mục với file `package.json` của bạn
 2. File `package.json` của bạn không được chứa thuộc tính `ava` (hoặc nếu có, nó phải là một đối tượng trống)

File cấu hình phải có một export mặc định, sử dụng các module ES. Nó có thể là một đối tượng đơn giản, hoặc một hàm factory nào đó trả về một đối tượng đơn giản:

```js
export default {
	require: ['esm']
};
```

```js
export default function factory() {
	return {
		require: ['esm']
	};
};
```

Hàm factory được gọi với một đối tượng chứa thuộc tính `projectDir`, thứ mà bạn có thể sử dụng để thay đổi cấu hình trả về:

```js
export default ({projectDir}) => {
	if (projectDir === '/Users/username/projects/my-project') {
		return {
			// Config A
		};
	}

	return {
		// Config B
	};
};
```

Lưu ý rằng cấu hình cuối cùng không được là một promise.

## Tài liệu

Các test được thực hiện đồng thời. Bạn có thể chỉ định các test chạy đồng bộ và không động bộ. Các test được con là đồng bộ, trừ khi bạn trả về một Promise hoặc  [observable](https://github.com/zenparsing/zen-observable).

Chúng tôi *khuyến khích* sử dụng [async-functions](#hỗ-trợ-async-function). Chúng giúp cho code không đồng bộ súc tích và có thể đọc được, và chúng cũng ngầm trả về một promise thay bạn.

Nếu bạn không thể sử dụng promises hay observables, bạn nên kích hoạt "callback mode" bằng cách định nghĩa các test của mình với `test.cb([title], fn)`. Các test được khai bao theo cách này **phải** được kết thúc một cách thủ công với `t.end()`. Chế độ này chủ yếu được dùng để test các callback-style APIs. Tuy nhiên, chúng tôi đặc biệt khuyến khích bạn dùng [promisifying](https://github.com/sindresorhus/pify) callback-style APIs thay vì sử dụng "callback mode", vì nó giúp test chạy chính xác và dễ đọc hơn.

Bạn phải định nghĩa tất cả các test một cách đồng bộ. Chúng không thể được định nghĩa bên trong `setTimeout`, `setImmediate`, v.v.

AVA sẽ cố gắng chạy các file test với thư mục làm việc hiện tại của chúng có chứa file `package.json`.

### Tạo các test

Để tạo một test bạn phải gọi hàm `test` bạn đã import vào từ AVA. Cung cấp tiêu đề yêu cầu và hàm chức năng cho nó. Hàm chức năng sẽ được gọi khi bạn chạy test của mình. Nó sẽ truyền vào [execution object](#t) là đối số đầu tiên của nó.

**Lưu ý:** Để [các thông báo xác nhận nâng cao](#thông-báo-xác-nhận-nâng-cao) hoạt động chính xác, đối số đầu tiên **phải** được đặt tên là `t`.

```js
import test from 'ava';

test('my passing test', t => {
	t.pass();
});
```

### Lập kế hoạch xác nhận

Các kế hoạch xác nhận phải đảm bảo rằng các test chỉ thành công khi một một số nhất định các kiểm thử giá trị đã được thực thi. Chúng sẽ giúp bạn nắm bắt các trường hợp test kết thúc quá sớm. Chúng cũng sẽ làm cho test thất bại nếu có quá nhiều kiểm thử giá trị được thực thi, điều này sẽ hữu ích nếu bạn có các xác nhận bên trong các callback hoặc các vòng lặp.

Nếu bạn không chỉ định rõ ràng một kế hoạch xác nhận, test của bạn vẫn sẽ thất bại nếu không có bất cứ xác nhận nào được thực thi. Thiết lập tùy chọn `failWithoutAssertions` thành `false` trong AVA's [Cấu hình `package.json`](#cấu-hình) để vô hiệu hóa hành vi này.

Lưu ý rằng, không giống như [`tap`](https://www.npmjs.com/package/tap) và [`tape`](https://www.npmjs.com/package/tape), AVA *không* tự động kết thúc một test khi đã đạt được số kế hoạch xác nhận dự kiến.

Các ví dụ sau đây sẽ có kết quả là một test đã thành công:

```js
test('resolves with 3', t => {
	t.plan(1);

	return Promise.resolve(3).then(n => {
		t.is(n, 3);
	});
});

test.cb('invokes callback', t => {
	t.plan(1);

	someAsyncFunction(() => {
		t.pass();
		t.end();
	});
});
```

Những ví dụ này thì không:

```js
test('loops twice', t => {
	t.plan(2);

	for (let i = 0; i < 3; i++) {
		t.true(i < 3);
	}
}); // Thất bại, 3 kiểm thử đã được thực thi, được xem là quá nhiều

test('invokes callback synchronously', t => {
	t.plan(1);

	someAsyncFunction(() => {
		t.pass();
	});
}); // Thất bại, test kết thúc một cách đồng bộ trước khi kiểm thử được thực thi
```

### Thực thi các test theo từng kỳ

Theo mặc định, các test được chạy đồng thời, tuy nhiên, đôi khi bạn phải viết các test không thể chạy đồng thời. Trong các trường hợp hiếm hoi này, bạn có thể dùng  `.serial`. Nó sẽ ép buộc các bài kiểm tra đó chạy ngay lập tức *trước* các test chạy đồng thời.

```js
test.serial('passes serially', t => {
	t.pass();
});
```

Lưu ý rằng điều này chỉ áp dụng cho các test trong một file test nhất định. AVA vẫn sẽ chạy nhiều file test cùng lúc trừ khi bạn truyền vào tham số [`--serial` CLI flag](#cli).

Bạn có thể sử dụng `.serial` với tất cả các test, hook hoặc tập chí `.todo()`, nhưng nó chỉ có sẵn trên hàm `test`.

### Thực thi một test cụ thể

Trong quá trình phát triển, đôi khi chỉ chạy một vài test cụ thể lại hữu ích. Điều này có thể được thực hiện bằng cách dùng `.only`:

```js
test('will not be run', t => {
	t.fail();
});

test.only('will be run', t => {
	t.pass();
});
```

Bạn có thể sử dụng `.only` với tất cả các test. Nó không thể được dùng với các hook hoặc `.todo()`.

*Lưu ý:* `.only` chỉ áp dụng cho tệp đã được xác định, vì vậy nếu bạn chạy nhiều test cùng lúc, các test ở các file khác vẫn sẽ chạy. Nếu bạn chỉ muốn thực thi một test `test.only`, chỉ cần cung cấp file test đó cho AVA.

### Thực thi test với các tiêu đề trùng khớp

Tham số `--match` cho phép bạn chạy các test có tiêu đề trùng khớp, Điều này có thể thực hiện bằng cách sử dụng một wildcard patterns đơn giản. Các patterns không phân biệt chữ hoa thường. Xem [`matcher`](https://github.com/sindresorhus/matcher) để có thêm thông tin.

Các tiêu đề trùng khớp kết thúc với `foo`:

```console
$ ava --match='*foo'
```

Các tiêu đề trùng khớp bắt đầu với `foo`:

```console
$ ava --match='foo*'
```

Các tiêu đề trùng khớp có chứa `foo`:

```console
$ ava --match='*foo*'
```

Các tiêu đề trùng khớp là `foo` (mặc dù không phân biệt chữ hoa thường):

```console
$ ava --match='foo'
```

Các tiêu đề trùng khớp không có chứa `foo`:

```console
$ ava --match='!*foo*'
```

Các tiêu đề trùng khớp bắt đầu với `foo` và kết thúc với `bar`:

```console
$ ava --match='foo*bar'
```

Các tiêu đề trùng khớp bắt đầu với `foo` hoặc kết thúc với `bar`:

```console
$ ava --match='foo*' --match='*bar'
```

Lưu ý rằng một pattern đối sánh được ưu tiên hơn `.only`. Chỉ các test có tiêu đề rõ ràng mới được đối sánh. Các test không có tiêu đề hoặc có tiêu đề bắt nguồn từ hàm thực thi sẽ bị bỏ qua khi `--match` được sử dụng.

Đây là những gì sẽ xảy ra nếu bạn chạy AVA với một pattern của `*oo*` và các test:

```js
test('foo will run', t => {
	t.pass();
});

test('moo will also run', t => {
	t.pass();
});

test.only('boo will run but not exclusively', t => {
	t.pass();
});

// Không chạy, không có tiêu đề
test(function (t) {
	t.fail();
});

// Không chạy, không có tiêu đề rõ ràng
test(function foo(t) {
	t.fail();
});
```

### Bỏ qua test

Đôi khi việc kiểm tra thất bại khó thể khó khắc phục. Bạn có thể yêu cầu AVA bỏ qua các test này bằng cách sử dụng `.skip`. Chúng sẽ vẫn được hiển thị trong output (là đã bị bỏ qua) nhưng không bao giờ thực thi.

```js
test.skip('will not be run', t => {
	t.fail();
});
```

Bạn phải chỉ định hàm thực thi. Bạn có thể sử dụng `.skip` cho tất cả các test và hook, nhưng không thể dùng với `.todo()`. Bạn không thể áp dụng các modifiers khác với `.skip`

### Test placeholders ("todo")

Bạn có thể dùng `.todo` khi bạn định viết một test. Như khi các test được bỏ qua, các placeholders này sẽ được hiển thị trong output. Chúng chỉ cần có một tiêu đề; bạn không thể chỉ định hàm thực thi.

```js
test.todo('will think about writing this later');
```

Bạn có thể báo hiệu rằng bạn cần phải viết một test nối tiếp:

```js
test.serial.todo('will think about writing this later');
```

### Test thất bại

Bạn có thể dùng modifier `.failing` để ghi lại các vấn đề với code của bạn mà bạn sẽ cần phải chỉnh sửa. Test thất bại được thực thi giống như các test thông thường, và sẽ không phá vỡ build của bạn khi chúng thực hiện. Nếu một test được đánh dấu là không thực sự thành công, nó sẽ được báo cáo như là một lỗi và làm thất bại build với một thông báo hướng dẫn hứu ích giúp bạn loại bỏ modifier `.failing`.

Điều này cho phép bạn kết hợp các test  `.failing` trước khi một bản sửa lỗi được thêm vào mà không phá vỡ CI. Đây là một cách tuyệt vời để nhận ra các báo cáo lỗi tốt của các PR với một commit tin cậy, ngay cả khi người báo cáo không thể thực sự khắc phục vấn đề.

```js
// Xem: github.com/user/repo/issues/1234
test.failing('demonstrate some bug', t => {
	t.fail(); // Test sẽ được tính là thành công
});
```

### Các hook trước và sau

AVA cho phép bạn đăng ký các hook được chạy trước vào sau test. Điều này cho phép bạn setup và/hoặc teardown code.

`test.before()` đăng ký một hook sẽ được chạy trước test đầu tiên trong file test của bạn. Tương tự `test.after()` đăng ký một hook sẽ chạy sau test cuối cùng. Sử dụng `test.after.always ()` để đăng ký mộc hook sẽ **luôn luôn** chạy sau khi test của bạn và các hook hoàn thành. Hook `.always()` sẽ chạy bất kể có lỗi trước đó hay không, vì vậy chúng rất lý tưởng cho các nhiệm vụ dọn dẹp. Tuy nhiên, lưu ý rằng các trường hợp ngoại lệ chưa được thực hiện, các rejection hoặc timeout sẽ phá hỏng test của bạn, thậm chí có thể ngăn cản hook `.always()` thực thi.

`test.beforeEach()` đăng ký một hook sẽ chạy trước mỗi test trong file test của bạn. Tương tự `test.afterEach()` sẽ đăng ký một hook sẽ chạy sau mỗi test. Sử dụng `test.afterEach.always()` để đăng ký một hook sẽ được gọi ngay cả khi các hook hoặc bản thân test đó thất bại.

Nếu một test bị bỏ qua với `.skip`, các hook  `.beforeEach()`, `.afterEach()` và `.afterEach.always()` sẽ không được thực thi. Tương tự vậy, nếu tất cả test trong một file bị bỏ qua thì các hook `.before()`, `.after()` và `.after.always()` sẽ không được thực thi.

Giống như `test()`, các phương thức này nhận vào một tiêu đề và một hàm thực thi. Tiêu đề sẽ được hiển thị nếu hook của bạn không thực thi được. Hàm thực thi sẽ được gọi với  một [execution object](#t). Bạn có thể sử dụng các xác nhận giá trị trong các hook của mình. Bạn cũng có thể truyền vào một [hàm macro](#test-macros) và các đối số bổ sung.

Các hook `.before()` thực thi trước các hook `.beforeEach()`. Các hook `.afterEach()` thực thi trước các hook `.after()`. Các hook sẽ thực thi theo thứ tự chúng được định nghĩa, nhưng bạn có thể dùng `test.serial` để đảm bảo rằng chỉ có một hook chạy ở một thời điểm. Không giống như test, các hook tuần tự *không* chạy trước các hook khác:

```js
test.before(t => {
	// Lệnh ở đây sẽ chạy trước tất cả các test
});

test.before(t => {
	// Lệnh ở đây sẽ chạy đồng thời với lệnh bên trên
});

test.serial.before(t => {
	// Lệnh ở đây sẽ chạy sau lệnh ở trên
});

test.serial.before(t => {
	// Lệnh ở đây cũng vậy, sẽ chạy sau các lệnh ở trên, và trước các test
});

test.after('cleanup', t => {
	// Lệnh ở đây chạy sau tất cả các test
});

test.after.always('guaranteed cleanup', t => {
	// Lệnh ở đây sẽ luôn chạy, bất kể các lỗi trước đó
});

test.beforeEach(t => {
	// Lệnh ở đây sẽ chạy trước mỗi test
});

test.afterEach(t => {
	// Lệnh ở đây sẽ chạy sau mỗi test
});

test.afterEach.always(t => {
	// Lệnh ở đây chạy sau mỗi test và các hook khác, ngay cả khi chúng thất bại
});

test('title', t => {
	// Test thông thường
});
```

Hook có thể đồng bộ hoặc bất đồng bộ giống như test. Để làm cho một hook bất đồng bộ trả về một promise hoặc observable, sử dụng một hàm async, hoặc kích hoạt chế độ callback bằng `test.before.cb()`, `test.beforeEach.cb()` .v.v

```js
test.before(async t => {
	await promiseFn();
});

test.after(t => {
	return new Promise(/* ... */);
});

test.beforeEach.cb(t => {
	setTimeout(t.end);
});

test.afterEach.cb(t => {
	setTimeout(t.end);
});
```

Hãy nhớ rằng các hook `.beforeEach()` à `.afterEach()` chạy ngay trước vào sau khi test được thực thi, và theo mặc định các test được chạy đồng thời. Điều này có nghĩa là  mỗi hook `.beforeEach()` có thể chạy đồng thời. Sử dụng `test.serial.beforeEach()` không thay đổi điều này. Nếu bạn cần thiết lập trạng thái toàn cục cho mỗi test (Như theo dõi `console.log` [ví dụ](https://github.com/avajs/ava/issues/560)), bạn sẽ cần phải đảm bảo rằng bản thân các test phải [chạy theo từng kỳ](#thực-thi-các-test-theo-từng-kỳ).

Hãy nhớ rằng AVA chạy mỗi file test trong tiến trình của chính nó. Bạn có thể sẽ không cần phải dọn dẹp lại trạng thái toàn cục của test với hook `.after()` vì nó chỉ được gọi ngay khi tiến trình kết thúc.

#### Test context

Hooks can share context with the test:

```js
test.beforeEach(t => {
	t.context.data = generateUniqueData();
});

test('context data is foo', t => {
	t.is(t.context.data + 'bar', 'foobar');
});
```

Context created in `.before()` hooks is [cloned](https://www.npmjs.com/package/lodash.clone) before it is passed to `.beforeEach()` hooks and / or tests. The `.after()` and `.after.always()` hooks receive the original context value.

For `.beforeEach()`, `.afterEach()` and `.afterEach.always()` hooks the context is *not* shared between different tests, allowing you to set up data such that it will not leak to other tests.

By default `t.context` is an object but you can reassign it:

```js
test.before(t => {
	t.context = 'unicorn';
});

test('context is unicorn', t => {
	t.is(t.context, 'unicorn');
});
```

### Test macros

Additional arguments passed to the test declaration will be passed to the test implementation. This is useful for creating reusable test macros.

```js
function macro(t, input, expected) {
	t.is(eval(input), expected);
}

test('2 + 2 = 4', macro, '2 + 2', 4);
test('2 * 3 = 6', macro, '2 * 3', 6);
```

You can build the test title programmatically by attaching a `title` function to the macro:

```js
function macro(t, input, expected) {
	t.is(eval(input), expected);
}

macro.title = (providedTitle, input, expected) => `${providedTitle} ${input} = ${expected}`.trim();

test(macro, '2 + 2', 4);
test(macro, '2 * 3', 6);
test('providedTitle', macro, '3 * 3', 9);
```

The `providedTitle` argument defaults to an empty string if the user does not supply a string title. This allows for easy concatenation without having to worry about `null` / `undefined`. It is worth remembering that the empty string is considered a falsy value, so you can still use `if(providedTitle) {...}`.

You can also pass arrays of macro functions:

```js
const safeEval = require('safe-eval');

function evalMacro(t, input, expected) {
	t.is(eval(input), expected);
}

function safeEvalMacro(t, input, expected) {
	t.is(safeEval(input), expected);
}

test([evalMacro, safeEvalMacro], '2 + 2', 4);
test([evalMacro, safeEvalMacro], '2 * 3', 6);
```

We encourage you to use macros instead of building your own test generators ([here is an example](https://github.com/avajs/ava-codemods/blob/47073b5b58aa6f3fb24f98757be5d3f56218d160/test/ok-to-truthy.js#L7-L9) of code that should be replaced with a macro). Macros are designed to perform static analysis of your code, which can lead to better performance, IDE integration, and linter rules.

### Custom assertions

You can use any assertion library instead of or in addition to the built-in one, provided it throws exceptions when the assertion fails.

This won't give you as nice an experience as you'd get with the [built-in assertions](#assertions) though, and you won't be able to use the [assertion planning](#assertion-planning) ([see #25](https://github.com/avajs/ava/issues/25)).

You'll have to configure AVA to not fail tests if no assertions are executed, because AVA can't tell if custom assertions pass. Set the `failWithoutAssertions` option to `false` in AVA's [`package.json` configuration](#configuration).

```js
import assert from 'assert';

test('custom assertion', t => {
	assert(true);
});
```

### Latest JavaScript support

AVA uses [Babel 7](https://babeljs.io) so you can use the latest JavaScript syntax in your tests. There is no extra setup required. You don't need to be using Babel in your own project for this to work either.

We aim support all [finished syntax proposals](https://github.com/tc39/proposals/blob/master/finished-proposals.md), as well as all syntax from ratified JavaScript versions (e.g. ES2017). See our [`@ava/stage-4`](https://github.com/avajs/babel-preset-stage-4) preset for the currently supported proposals.

Please note that we do not add or modify built-ins. For example, if you use [`Object.entries()`](https://github.com/tc39/proposal-object-values-entries) in your tests, they will crash in Node.js 6 which does not implement this method.

You can disable this syntax support, or otherwise customize AVA's Babel pipeline. See our [Babel recipe] for more details.

### TypeScript support

AVA includes typings for TypeScript. You have to set up transpilation yourself. When you set `module` to `commonjs` in your `tsconfig.json` file, TypeScript will automatically find the type definitions for AVA. You should set `target` to `es2015` to use promises and async functions.

See AVA's [TypeScript recipe](docs/recipes/typescript.md) for a more detailed explanation.

### Transpiling imported modules

AVA currently only transpiles the tests you ask it to run, as well as test helpers (files starting with `_` or in `helpers` directory) inside the test directory. *It will not transpile modules you `import` from outside of the test.* This may be unexpected but there are workarounds.

If you use Babel you can use its [require hook](https://babeljs.io/docs/usage/require/) to transpile imported modules on-the-fly. To add it, [configure it in your `package.json`](#configuration).

You can also transpile your modules in a separate process and refer to the transpiled files rather than the sources from your tests. Example [here](docs/recipes/precompiling-with-webpack.md).

### Promise support

If you return a promise in the test you don't need to explicitly end the test as it will end when the promise resolves.

```js
test('resolves with unicorn', t => {
	return somePromise().then(result => {
		t.is(result, 'unicorn');
	});
});
```

### Async function support

AVA comes with built-in support for [async functions](https://tc39.github.io/ecmascript-asyncawait/) *(async/await)*.

```js
test(async function (t) {
	const value = await promiseFn();
	t.true(value);
});

// Async arrow function
test('promises the truth', async t => {
	const value = await promiseFn();
	t.true(value);
});
```

### Observable support

AVA comes with built-in support for [observables](https://github.com/zenparsing/es-observable). If you return an observable from a test, AVA will automatically consume it to completion before ending the test.

*You do not need to use "callback mode" or call `t.end()`.*

```js
test('handles observables', t => {
	t.plan(3);
	return Observable.of(1, 2, 3, 4, 5, 6)
		.filter(n => {
			// Only even numbers
			return n % 2 === 0;
		})
		.map(() => t.pass());
});
```

### Callback support

AVA supports using `t.end` as the final callback when using node-style error-first callback APIs. AVA will consider any truthy value passed as the first argument to `t.end` to be an error. Note that `t.end` requires "callback mode", which can be enabled by using the `test.cb` chain.

```js
test.cb('data.txt can be read', t => {
	// `t.end` automatically checks for error as first argument
	fs.readFile('data.txt', t.end);
});
```

### Global timeout

A global timeout can be set via the `--timeout` option. Timeout in AVA behaves differently than in other test frameworks. AVA resets a timer after each test, forcing tests to quit if no new test results were received within the specified timeout. This can be used to handle stalled tests.

You can set timeouts in a human-readable way:

```console
$ ava --timeout=10s # 10 seconds
$ ava --timeout=2m # 2 minutes
$ ava --timeout=100 # 100 milliseconds
```

### Parallel runs in CI

AVA automatically detects whether your CI environment supports parallel builds. Each build will run a subset of all test files, while still making sure all tests get executed. See the [`ci-parallel-vars`](https://www.npmjs.com/package/ci-parallel-vars) package for a list of supported CI environments.

## API

### `test([title], implementation)`
### `test.serial([title], implementation)`
### `test.cb([title], implementation)`
### `test.only([title], implementation)`
### `test.skip([title], implementation)`
### `test.todo(title)`
### `test.failing([title], implementation)`
### `test.before([title], implementation)`
### `test.after([title], implementation)`
### `test.beforeEach([title], implementation)`
### `test.afterEach([title], implementation)`

#### `title`

Type: `string`

Test title.

#### `implementation(t)`

Type: `function`

Should contain the actual test.

##### `t`

Type: `object`

The execution object of a particular test. Each test implementation receives a different object. Contains the [assertions](#assertions) as well as `.plan(count)` and `.end()` methods. `t.context` can contain shared state from hooks. `t.title` returns the test's title.

###### `t.plan(count)`

Plan how many assertion there are in the test. The test will fail if the actual assertion count doesn't match the number of planned assertions. See [assertion planning](#assertion-planning).

###### `t.end()`

End the test. Only works with `test.cb()`.

###### `t.log(...values)`

Log values contextually alongside the test result instead of immediately printing them to `stdout`. Behaves somewhat like `console.log`, but without support for placeholder tokens.

## Assertions

Assertions are mixed into the [execution object](#t) provided to each test implementation:

```js
test('unicorns are truthy', t => {
	t.truthy('unicorn'); // Assertion
});
```

Assertions are bound to their test so you can assign them to a variable or pass them around:

```js
test('unicorns are truthy', t => {
	const truthy = t.truthy;
	truthy('unicorn');
});
```

Assertions can be skipped by adding `.skip()`:

```js
test('unicorns are truthy', t => {
	t.truthy.skip('unicorn');
});
```

If multiple assertion failures are encountered within a single test, AVA will only display the *first* one.

### `.pass([message])`

Passing assertion.

### `.fail([message])`

Failing assertion.

### `.truthy(value, [message])`

Assert that `value` is truthy.

### `.falsy(value, [message])`

Assert that `value` is falsy.

### `.true(value, [message])`

Assert that `value` is `true`.

### `.false(value, [message])`

Assert that `value` is `false`.

### `.is(value, expected, [message])`

Assert that `value` is the same as `expected`. This is based on [`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is).

### `.not(value, expected, [message])`

Assert that `value` is not the same as `expected`. This is based on [`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is).

### `.deepEqual(value, expected, [message])`

Assert that `value` is deeply equal to `expected`. See [Concordance](https://github.com/concordancejs/concordance) for details. Works with [React elements and `react-test-renderer`](https://github.com/concordancejs/react).

### `.notDeepEqual(value, expected, [message])`

Assert that `value` is not deeply equal to `expected`. The inverse of `.deepEqual()`.

### `.throws(thrower, [expected, [message]])`

Assert that an error is thrown. `thrower` can be a function which should throw, or return a promise that should reject. Alternatively a promise can be passed directly.

The thrown value *must* be an error. It is returned so you can run more assertions against it.

`expected` can be a constructor, in which case the thrown error must be an instance of the constructor. It can be a string, which is compared against the thrown error's message, or a regular expression which is matched against this message. You can also specify a matcher object with one or more of the following properties:

* `instanceOf`: a constructor, the thrown error must be an instance of
* `is`: the thrown error must be strictly equal to `expected.is`
* `message`: either a string, which is compared against the thrown error's message, or a regular expression, which is matched against this message
* `name`: the expected `.name` value of the thrown error
* `code`: the expected `.code` value of the thrown error

`expected` does not need to be specified. If you don't need it but do want to set an assertion message you have to specify `null`.

Example:

```js
const fn = () => {
	throw new TypeError('🦄');
};

test('throws', t => {
	const error = t.throws(() => {
		fn();
	}, TypeError);

	t.is(error.message, '🦄');
});
```

```js
const promise = Promise.reject(new TypeError('🦄'));

test('rejects', async t => {
	const error = await t.throws(promise);
	t.is(error.message, '🦄');
});
```

When testing a promise you must wait for the assertion to complete:

```js
test('rejects', async t => {
	await t.throws(promise);
});
```

When testing an asynchronous function you must also wait for the assertion to complete:

```js
test('throws', async t => {
	await t.throws(async () => {
		throw new TypeError('🦄');
	}, {instanceOf: TypeError, message: '🦄'});
});
```

### `.notThrows(nonThrower, [message])`

Assert that no error is thrown. `thrower` can be a function which shouldn't throw, or return a promise that should resolve. Alternatively a promise can be passed directly.

Like the `.throws()` assertion, when testing a promise you must wait for the assertion to complete:

```js
test('resolves', async t => {
	await t.notThrows(promise);
});
```

### `.regex(contents, regex, [message])`

Assert that `contents` matches `regex`.

### `.notRegex(contents, regex, [message])`

Assert that `contents` does not match `regex`.

### `.snapshot(expected, [message])`
### `.snapshot(expected, [options], [message])`

Compares the `expected` value with a previously recorded snapshot. Snapshots are stored for each test, so ensure you give your tests unique titles. Alternatively pass an `options` object to select a specific snapshot, for instance `{id: 'my snapshot'}`.

Snapshot assertions cannot be skipped when snapshots are being updated.

## Snapshot testing

AVA supports snapshot testing, [as introduced by Jest](https://facebook.github.io/jest/docs/snapshot-testing.html), through its [Assertions](#assertions) interface. You can snapshot any value as well as React elements:

```js
// Your component
const HelloWorld = () => <h1>Hello World...!</h1>;

export default HelloWorld;
```

```js
// Your test
import test from 'ava';
import render from 'react-test-renderer';
import HelloWorld from '.';

test('HelloWorld component', t => {
	const tree = render.create(<HelloWorld/>).toJSON();
	t.snapshot(tree);
});
```

[Try it out in this example project.](https://github.com/avajs/ava-snapshot-example)

Snapshots are stored alongside your test files. If your tests are in a `test` or `tests` folder the snapshots will be stored in a `snapshots` folder. If your tests are in a `__tests__` folder then they they'll be stored in a `__snapshots__` folder.

Say you have `~/project/test/main.js` which contains snapshot assertions. AVA will create two files:

* `~/project/test/snapshots/main.js.snap`
* `~/project/test/snapshots/main.js.md`

The first file contains the actual snapshot and is required for future comparisons. The second file contains your *snapshot report*. It's regenerated when you update your snapshots. If you commit it to source control you can diff it to see the changes to your snapshot.

AVA will show why your snapshot assertion failed:

<img src="media/snapshot-testing.png" width="1048">

You can then check your code. If the change was intentional you can use the `--update-snapshots` (or `-u`) flag to update the snapshots:

```console
$ ava --update-snapshots
```

You can specify a fixed location for storing the snapshot files in AVA's [`package.json` configuration](#configuration):

```json
{
	"ava": {
		"snapshotDir": "custom-directory"
	}
}
```

The snapshot files will be saved in a directory structure that mirrors that of your test files.

If you are running AVA against precompiled test files, AVA will try and use source maps to determine the location of the original files. Snapshots will be stored next to these files, following the same rules as if AVA had executed the original files directly. This is great if you're writing your tests in TypeScript (see our [TypeScript recipe](docs/recipes/typescript.md)).

### Skipping assertions

Any assertion can be skipped using the `skip` modifier. Skipped assertions are still counted, so there is no need to change your planned assertion count.

```js
test('skip assertion', t => {
	t.plan(2);
	t.is.skip(foo(), 5); // No need to change your plan count when skipping
	t.is(1, 1);
});
```

### Enhanced assertion messages

AVA comes with [`power-assert`](https://github.com/power-assert-js/power-assert) built-in, giving you more descriptive assertion messages. It reads your test and tries to infer more information from the code.

Let's take this example, using Node's standard [`assert` library](https://nodejs.org/api/assert.html):

```js
const a = /foo/;
const b = 'bar';
const c = 'baz';
require('assert').ok(a.test(b) || b === c);
```

If you paste that into a Node REPL it'll return:

```
AssertionError: false == true
```

In AVA however, this test:

```js
test('enhanced assertions', t => {
	const a = /foo/;
	const b = 'bar';
	const c = 'baz';
	t.true(a.test(b) || b === c);
});
```

Will output:

```
t.true(a.test(b) || b === c)
       |      |     |     |
       |      "bar" "bar" "baz"
       false
```

## Process isolation

Each test file is run in a separate Node.js process. This allows you to change the global state or overriding a built-in in one test file, without affecting another. It's also great for performance on modern multi-core processors, allowing multiple test files to execute in parallel.

AVA will set `process.env.NODE_ENV` to `test`, unless the `NODE_ENV` environment variable has been set. This is useful if the code you're testing has test defaults (for example when picking what database to connect to, or environment-specific Babel options). It may cause your code or its dependencies to behave differently though. Note that `'NODE_ENV' in process.env` will always be `true`.

## Tips

### Temp files

Running tests concurrently comes with some challenges, doing file IO is one.

Usually, serial tests create temp directories in the current test directory and clean them up at the end. This won't work when you run tests concurrently as tests will conflict with each other. The correct way to do it is to use a new temp directory for each test. The [`tempfile`](https://github.com/sindresorhus/tempfile) and [`temp-write`](https://github.com/sindresorhus/temp-write) modules can be helpful.

### Code coverage

You can't use [`istanbul`](https://github.com/gotwarlost/istanbul) for code coverage as AVA [spawns the test files](#process-isolation). You can use [`nyc`](https://github.com/bcoe/nyc) instead, which is basically `istanbul` with support for subprocesses.

As of version `5.0.0` it uses source maps to report coverage for your actual code, regardless of transpilation. Make sure that the code you're testing includes an inline source map or references a source map file. If you use `babel-register` you can set the `sourceMaps` option in your Babel config to `inline`.

### Common pitfalls

We have a growing list of [common pitfalls](docs/common-pitfalls.md) you may experience while using AVA. If you encounter any issues you think are common, comment in [this issue](https://github.com/avajs/ava/issues/404).

## FAQ

### Why not `mocha`, `tape`, `tap`?

Mocha requires you to use implicit globals like `describe` and `it` with the default interface (which most people use). It's not very opinionated and executes tests serially without process isolation, making it slow.

Tape and tap are pretty good. AVA is highly inspired by their syntax. They too execute tests serially. Their default [TAP](https://testanything.org) output isn't very user-friendly though so you always end up using an external tap reporter.

In contrast AVA is highly opinionated and runs tests concurrently, with a separate process for each test file. Its default reporter is easy on the eyes and yet AVA still supports TAP output through a CLI flag.

### How is the name written and pronounced?

AVA, not Ava or ava. Pronounced [/ˈeɪvə/ ay-və](media/pronunciation.m4a?raw=true).

### What is the header background?

It's the [Andromeda galaxy](https://simple.wikipedia.org/wiki/Andromeda_galaxy).

### What is the difference between concurrency and parallelism?

[Concurrency is not parallelism. It enables parallelism.](https://stackoverflow.com/q/1050222)

## Recipes

- [Test setup](docs/recipes/test-setup.md)
- [Code coverage](docs/recipes/code-coverage.md)
- [Watch mode](docs/recipes/watch-mode.md)
- [Endpoint testing](docs/recipes/endpoint-testing.md)
- [When to use `t.plan()`](docs/recipes/when-to-use-plan.md)
- [Browser testing](docs/recipes/browser-testing.md)
- [TypeScript](docs/recipes/typescript.md)
- [Flow](docs/recipes/flow.md)
- [Configuring Babel][Babel recipe]
- [Using ES modules](docs/recipes/es-modules.md)
- [Passing arguments to your test files](docs/recipes/passing-arguments-to-your-test-files.md)
- [Testing React components](docs/recipes/react.md)
- [Testing Vue.js components](docs/recipes/vue.md)
- [JSPM and SystemJS](docs/recipes/jspm-systemjs.md)
- [Debugging tests with Chrome DevTools](docs/recipes/debugging-with-chrome-devtools.md)
- [Debugging tests with WebStorm](docs/recipes/debugging-with-webstorm.md)
- [Precompiling source files with webpack](docs/recipes/precompiling-with-webpack.md)
- [Isolated MongoDB integration tests](docs/recipes/isolated-mongodb-integration-tests.md)

## Support

- [Stack Overflow](https://stackoverflow.com/questions/tagged/ava)
- [Spectrum](https://spectrum.chat/ava)
- [Twitter](https://twitter.com/ava__js)

## Related

- [eslint-plugin-ava](https://github.com/avajs/eslint-plugin-ava) - Lint rules for AVA tests
- [sublime-ava](https://github.com/avajs/sublime-ava) - Snippets for AVA tests
- [atom-ava](https://github.com/avajs/atom-ava) - Snippets for AVA tests
- [vscode-ava](https://github.com/samverschueren/vscode-ava) - Snippets for AVA tests
- [gulp-ava](https://github.com/avajs/gulp-ava) - Run tests with gulp
- [grunt-ava](https://github.com/avajs/grunt-ava) - Run tests with grunt
- [More…](https://github.com/avajs/awesome-ava#packages)

## Links

- [AVA stickers, t-shirts, etc](https://www.redbubble.com/people/sindresorhus/works/30330590-ava-logo)
- [Awesome list](https://github.com/avajs/awesome-ava)
- [AVA Casts](http://avacasts.com)
- [More…](https://github.com/avajs/awesome-ava)

## Team

[![Mark Wubben](https://github.com/novemberborn.png?size=100)](https://github.com/novemberborn) | [![Sindre Sorhus](https://github.com/sindresorhus.png?size=100)](https://github.com/sindresorhus) | [![Vadim Demedes](https://github.com/vadimdemedes.png?size=100)](https://github.com/vadimdemedes)
---|---|---
[Mark Wubben](https://novemberborn.net) | [Sindre Sorhus](http://sindresorhus.com) | [Vadim Demedes](https://github.com/vadimdemedes)

###### Former

- [Kevin Mårtensson](https://github.com/kevva)
- [James Talmage](https://github.com/jamestalmage)
- [Juan Soto](https://github.com/sotojuan)
- [Jeroen Engels](https://github.com/jfmengels)


<div align="center">
	<br>
	<br>
	<br>
	<a href="https://ava.li">
		<img src="media/logo.svg" width="200" alt="AVA">
	</a>
	<br>
	<br>
</div>

[Babel recipe]: docs/recipes/babel.md
