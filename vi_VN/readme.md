___
**Lưu ý của người phiên dịch**

Đây là bản dịch của file [readme.md](https://github.com/avajs/ava/blob/master/readme.md). Đây là [liên kết]() để thấy sự khác biệt với nhánh chính của AVA (Nếu bạn nhấp vào liên kết mà không thấy có sự thay đổi đối với file `readme.md` thì có nghĩa là bản dịch đã được cập nhật).
___
# [![AVA](media/header.png)](https://ava.li)

> Test runner của tương lai 

[![Build Status: Linux](https://travis-ci.org/avajs/ava.svg?branch=master)](https://travis-ci.org/avajs/ava) [![Build status: Windows](https://ci.appveyor.com/api/projects/status/e7v91mu2m5x48ehx/branch/master?svg=true)](https://ci.appveyor.com/project/ava/ava/branch/master) [![Coverage Status](https://coveralls.io/repos/github/avajs/ava/badge.svg?branch=master)](https://coveralls.io/github/avajs/ava?branch=master) [![XO code style](https://img.shields.io/badge/code_style-XO-5ed9c7.svg)](https://github.com/xojs/xo) [![Join the community on Spectrum](https://withspectrum.github.io/badge/badge.svg)](https://spectrum.chat/ava)
 [![Mentioned in Awesome Node.js](https://awesome.re/mentioned-badge.svg)](https://github.com/sindresorhus/awesome-nodejs)

Mặc dù JavaScript là một ngôn ngữ đơn luồng, nhưng IO trong Node.js có thể thực thi song song do tính chất không đồng bộ của nó. AVA tận dụng lợi thế này để chạy các test của bạn một cách đồng thời, điều này đặc biệt có lợi cho việc thực thi các test IO cồng kềnh. Ngoài ra, các file test được chạy song song như các process riêng biệt, cho bạn hiệu năng tốt hơn và một môi trường độc lập cho mỗi file test. [Chuyển](https://github.com/sindresorhus/pageres/commit/663be15acb3dd2eb0f71b1956ef28c2cd3fdeed0) từ Mocha sang AVA trong dự án Pageres đã giảm thời gian chạy test từ 31 xuống còn 11 giây. Việc có các test chạy đồng thời buộc bạn phải viết từng test nhỏ, chi tiết, có ý nghĩa và không phụ thuộc vào trạng thái toàn cục hoặc trạng thái của các test khác, đó là một điều tuyệt vời!

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
    --match, -m             Chỉ chạy test với các tiêu đề phù hợp (Có thể lặp lại)
    --update-snapshots, -u  Cập nhật các snapshot
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

  Pattern mặc định khi không có tham số:
  test.js test-*.js test/**/*.js **/__tests__/**/*.js **/*.test.js
```

*Lưu ý rằng CLI sẽ sử dụng cài đặt cục bộ của AVA khi có thể, ngay cả khi bạn chạy lệnh toàn cục.*

Các thư mục được đệ quy, tất cả các file có đuôi `*.js` sẽ được xem như là file test. Các thư mục có tên `fixtures`, `helpers` và `node_modules` sẽ *luôn* bị bỏ qua. Các file có tên bắt đầu bằng `_` sẽ cho phép bạn đặt những helper code trong cùng thư mục với file test của bạn.

Khi sử dụng lệnh `npm test`, bạn có thể truyền vào trực tiếp các tham số để chỉ rõ file cần test `npm test test2.js`, nhưng các cờ nên được truyền vào như sau `npm test -- --verbose`.


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

Dùng cờ `--verbose` để kích hoạt báo cáo chi tiết. Báo cáo chi tiết sẽ luôn được dùng trong môi trường CI, trừ khi [Báo cáo TAP](#tap-reporter) đã được kích hoạt.

<img src="media/verbose-reporter.png" width="294">

### Báo cáo TAP

AVA hỗ trợ định dạng TAP và do đó tương thích với [Bất kì báo cáo TAP nào khác](https://github.com/sindresorhus/awesome-tap#reporters). Sử dụng cờ `--tap` để bật báo cáo TAP.

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

Các test được thực hiện đồng thời. Bạn có thể chỉ định các test chạy đồng bộ và không động bộ. Các test được coi là đồng bộ, trừ khi bạn trả về một Promise hoặc  [observable](https://github.com/zenparsing/zen-observable).

Chúng tôi *khuyến khích* sử dụng [async-functions](#hỗ-trợ-async-function). Chúng giúp cho code không đồng bộ súc tích hơn và có thể đọc được, và chúng cũng ngầm trả về một promise thay bạn.

Nếu bạn không thể sử dụng promises hay observables, bạn nên kích hoạt "callback mode" bằng cách định nghĩa các test của mình với `test.cb([title], fn)`. Các test được khai bao theo cách này **phải** được kết thúc một cách thủ công với `t.end()`. Chế độ này chủ yếu được dùng để test các callback-style APIs. Tuy nhiên, chúng tôi đặc biệt khuyến khích bạn dùng [promisifying](https://github.com/sindresorhus/pify) callback-style APIs thay vì sử dụng "callback mode", vì nó giúp test chạy chính xác và dễ đọc hơn.

Bạn phải định nghĩa tất cả các test một cách đồng bộ. Chúng không thể được định nghĩa bên trong `setTimeout`, `setImmediate`, v.v.

AVA sẽ cố gắng chạy các file test với thư mục làm việc hiện tại của chúng có chứa file `package.json`.

### Tạo các test

Để tạo một test bạn phải gọi hàm `test` bạn đã import vào từ AVA. Cung cấp tiêu đề và hàm thực thi bắt buộc cho nó. Hàm thực thi sẽ được gọi khi bạn chạy test của mình. Nó sẽ truyền vào [execution object](#t) là đối số đầu tiên của nó.

**Lưu ý:** Để [các thông báo xác nhận giá trị nâng cao](#thông-báo-xác nhận-giá-trị-nâng-cao) hoạt động chính xác, đối số đầu tiên **phải** được đặt tên là `t`.

```js
import test from 'ava';

test('my passing test', t => {
	t.pass();
});
```

### Lập kế hoạch xác nhận giá trị

Các kế hoạch xác nhận giá trị phải đảm bảo rằng các test chỉ thành công khi một một số nhất định các kiểm thử giá trị đã được thực thi. Chúng sẽ giúp bạn nắm bắt các trường hợp test kết thúc quá sớm. Chúng cũng sẽ làm cho test thất bại nếu có quá nhiều kiểm thử giá trị được thực thi, điều này sẽ hữu ích nếu bạn có các xác nhận giá trị bên trong các callback hoặc các vòng lặp.

Nếu bạn không chỉ định rõ ràng một kế hoạch xác nhận giá trị, test của bạn vẫn sẽ thất bại nếu không có bất cứ xác nhận giá trị nào được thực thi. Thiết lập tùy chọn `failWithoutAssertions` thành `false` trong AVA's [Cấu hình `package.json`](#cấu-hình) để vô hiệu hóa hành vi này.

Lưu ý rằng, không giống như [`tap`](https://www.npmjs.com/package/tap) và [`tape`](https://www.npmjs.com/package/tape), AVA *không* tự động kết thúc một test khi đã đạt được số kế hoạch xác nhận giá trị dự kiến.

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

### Thực thi các test theo thứ tự 

Theo mặc định, các test được chạy đồng thời, tuy nhiên, đôi khi bạn phải viết các test không thể chạy đồng thời. Trong các trường hợp hiếm hoi này, bạn có thể dùng  `.serial`. Nó sẽ ép buộc các xác nhận giá trị đó chạy ngay lập tức, *trước* các test chạy đồng thời.

```js
test.serial('passes serially', t => {
	t.pass();
});
```

Lưu ý rằng điều này chỉ áp dụng cho các test trong một file test nhất định. AVA vẫn sẽ chạy nhiều file test cùng lúc trừ khi bạn truyền vào cờ [`--serial` CLI flag](#cli).

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

Cờ `--match` cho phép bạn chạy các test có tiêu đề trùng khớp, Điều này có thể thực hiện bằng cách sử dụng một wildcard patterns đơn giản. Các pattern không phân biệt chữ hoa thường. Xem [`matcher`](https://github.com/sindresorhus/matcher) để có thêm thông tin.

Các tiêu đề kết thúc với `foo`:

```console
$ ava --match='*foo'
```

Các tiêu đề bắt đầu với `foo`:

```console
$ ava --match='foo*'
```

Các tiêu đề có chứa `foo`:

```console
$ ava --match='*foo*'
```

Các tiêu đề là `foo` (mặc dù không phân biệt chữ hoa thường):

```console
$ ava --match='foo'
```

Các tiêu đề không có chứa `foo`:

```console
$ ava --match='!*foo*'
```

Các tiêu đề bắt đầu với `foo` và kết thúc với `bar`:

```console
$ ava --match='foo*bar'
```

Các tiêu đề bắt đầu với `foo` hoặc kết thúc với `bar`:

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

Đôi khi việc xác nhận giá trị thất bại khó thể khó khắc phục. Bạn có thể yêu cầu AVA bỏ qua các test này bằng cách sử dụng `.skip`. Chúng sẽ vẫn được hiển thị trong output (là đã bị bỏ qua) nhưng không bao giờ thực thi.

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

Nếu một test bị bỏ qua với `.skip`, các hook  `.beforeEach()`, `.afterEach()` và `.afterEach.always()` sẽ không được thực thi. Tương tự như vậy, nếu tất cả test trong một file bị bỏ qua thì các hook `.before()`, `.after()` và `.after.always()` sẽ không được thực thi.

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

Hãy nhớ rằng các hook `.beforeEach()` và `.afterEach()` chạy ngay trước vào sau khi test được thực thi, và theo mặc định các test được chạy đồng thời. Điều này có nghĩa là  mỗi hook `.beforeEach()` có thể chạy đồng thời. Sử dụng `test.serial.beforeEach()` không thay đổi điều này. Nếu bạn cần thiết lập trạng thái toàn cục cho mỗi test (Như theo dõi `console.log` [ví dụ](https://github.com/avajs/ava/issues/560)), bạn sẽ cần phải đảm bảo rằng bản thân các test phải [chạy theo từng kỳ](#thực-thi-các-test-theo-từng-kỳ).

Hãy nhớ rằng AVA chạy mỗi file test trong process của chính nó. Bạn có thể sẽ không cần phải dọn dẹp lại trạng thái toàn cục của test với hook `.after()` vì nó chỉ được gọi ngay khi process kết thúc.

#### Test theo ngữ cảnh

Các hook có thể chia sẻ ngữ cảnh với test:

```js
test.beforeEach(t => {
	t.context.data = generateUniqueData();
});

test('context data is foo', t => {
	t.is(t.context.data + 'bar', 'foobar');
});
```

Ngữ cảnh được tạo ra bằng hook `.before()` được [cloned](https://www.npmjs.com/package/lodash.clone) trước khi nó được chuyển tới các hook `.beforeEach()` và / hoặc các test. Các hook `.after()` và `.after.always()` sẽ nhận được giá trị ngữ cảnh ban đầu.

Đối với các hook `.beforeEach()`, `.afterEach()` và `.afterEach.always()` thì ngữ cảnh sẽ *không* được chia sẻ giữa các test với nhau, giúp dữ liệu mà bạn thiết lập sẽ không bị rò rĩ giữa các test với nhau.

Theo mặc định `t.context` là một object, nhưng bạn có thể gán lại nó:

```js
test.before(t => {
	t.context = 'unicorn';
});

test('context is unicorn', t => {
	t.is(t.context, 'unicorn');
});
```

### Test macros

Các đối số được truyền vào định nghĩa của test sẽ được chuyển vào test. Điều này rất hữu ích để tạo ra các test macro.

```js
function macro(t, input, expected) {
	t.is(eval(input), expected);
}

test('2 + 2 = 4', macro, '2 + 2', 4);
test('2 * 3 = 6', macro, '2 * 3', 6);
```

Bạn có thể xây dựng tiêu đề của test bằng cách lập trình gắn một `tiêu đề` của hàm vào macro

```js
function macro(t, input, expected) {
	t.is(eval(input), expected);
}

macro.title = (providedTitle, input, expected) => `${providedTitle} ${input} = ${expected}`.trim();

test(macro, '2 + 2', 4);
test(macro, '2 * 3', 6);
test('providedTitle', macro, '3 * 3', 9);
```

Đối số `providedTitle` theo mặc định sẽ là một chuỗi rỗng nếu người dùng không cung cấp một tiêu đề dạng chuỗi. Điều này cho phép chúng ta dễ dàng có được sự liên kết mà không cần phải lo lắng về `null` / `undefined`. Điều đáng để ghi nhớ là một chuỗi rỗng sẽ được coi là một giá trị sai, vì vậy bạn vẫn có thể dùng `if(providedTitle) {...}`.

Bạn cũng có thể truyền vào các chuỗi là các hàm macro:

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

Chúng tôi khuyến khích bạn sử dụng macro thay vì xây dựng các test generator của riêng bạn ([đây là ví dụ](https://github.com/avajs/ava-codemods/blob/47073b5b58aa6f3fb24f98757be5d3f56218d160/test/ok-to-truthy.js#L7-L9) của một đoạn code nên thay bằng macro). Các macro được thiết kế để thực hiện việc phân tích code của bạn, nên nó có thể đem lại hiệu suất tốt hơn, tích hợp với IDE, và các quy tắc linter.

### Xác nhận giá trị tùy chọn

Bạn có thể sử dụng bất kì thư việc xác nhận giá trị (assertion) hoặc thư việc được tích hợp sẵn, miễn là nó throw các ngoại lệ khi xác nhận giá trị thất bại.

Điều này sẽ không mang lại cho bạn một trải nghiệm tốt so với việc sử dụng thư viện [xác nhận giá trị được tích hợp sẵn](#xác-nhận-giá-trị), và bạn sẽ không thể sử dụng [lập kế hoạch xác nhận giá trị](#lập-kế-hoạch-xác-nhận-giá-trị) ([xem #25](https://github.com/avajs/ava/issues/25)).

Bạn sẽ cần phải cấu hình AVA để nó không đánh giá các test là thất bại nếu không có xác nhận giá trị nào được thực thi, bởi vì AVA không thể xác định liệu xác nhận giá trị tùy chọn có thành công hay không. Hãy đặt tùy chọn `failWithoutAssertions` thành `false` trong [cấu hình `package.json`](#cấu-hình) của AVA.

```js
import assert from 'assert';

test('custom assertion', t => {
	assert(true);
});
```

### Hỗ trợ JavaScript mới nhất

AVA sử dụng [Babel 7](https://babeljs.io) vì vậy bạn có thể sử dụng các cú pháp JavaScript mới nhất trong test của mình. Không cần phải cấu hình gì thêm cả. Bạn cũng không cần phải sử dụng Babel trong dự án của mình.

Chúng tôi nhắm đến mục đích là sẽ hỗ trợ [finished syntax proposals](https://github.com/tc39/proposals/blob/master/finished-proposals.md), cũng như tất cả các cú pháp từ các phiên bản JavaScript khác nhau đã được phê duyệt (Ví dụ ES2017). Xem [`@ava/stage-4`](https://github.com/avajs/babel-preset-stage-4) được đặt trước cho các proposals được hỗ trợ.

Xin hãy lưu ý rằng chúng tôi không thêm hoặc sửa đổi các bản được tích hợp sẵn. Ví dụ, nếu bạn dùng [`Object.entries()`](https://github.com/tc39/proposal-object-values-entries) trong các test của bạn, chúng sẽ bị lỗi trong Node.js 6, vì nó không bao gồm phương thức này.

Bạn có thể vô hiệu hóa hỗ trợ cú pháp, hoặc tùy chỉnh Babel pipiline của AVA. Xem [Công thức Babel] để biết thêm chi tiết.

### Hỗ trợ TypeScript

AVA cũng hỗ trợ TypeScript. Bạn cần phải tự mình thiết lập transpilation. Khi bạn đặt `module` thành `commonjs` trong tệp `tsconfig.json` của bạn, TypeScript sẽ tự động tìm các định nghĩa type cho AVA. Bạn nên đặt `target` thành `es2015` để sử dụng promises và async functions.

Xem [Công thức TypeScript](docs/recipes/typescript.md) để có các giải thích chi tiết.

### Transpile các module được import

Hiện tại AVA chỉ transpile các test mà bạn yêu cầu chúng chạy, cũng như các test helper (file bắt đầu với `_` trong thư mục `helpers`) nằm trong thư mục test của bạn. *AVA sẽ không transpile các module bạn `import` từ bên ngoài test`.* Đây có thể là điều bạn không mong đợi nhưng chúng ta có vài cách để giải quyết.

Nếu bạn dùng Babel, bạn có thể sử dụng [require hook](https://babeljs.io/docs/usage/require/) của nó để chuyển đổi các module được import trực tiếp. Thể thêm nó, [Cài đặt nó vào `package.json`](#cài-đặt) của bạn

Bạn cũng có thể transpile các module của bạn trong một process và tham khảo các file đã transpile thay vì các sources từ test của bạn. Ví dụ [ở đây](docs/recipes/precompiling-with-webpack.md).

### Hỗ trợ Promise

Nếu bạn trả về một promise trong test của bạn, bạn không cần phải kết thúc test của mình một cách rõ ràng bởi vì test sẽ kết thúc khi promise được resolve.

```js
test('resolves with unicorn', t => {
	return somePromise().then(result => {
		t.is(result, 'unicorn');
	});
});
```

### Hỗ trợ async function

AVA đi kèm với hỗ trợ tích hợp cho [async functions](https://tc39.github.io/ecmascript-asyncawait/) *(async/await)*.

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

### Hỗ trợ Observable

AVA đi kèm với hỗ trợ tích hợp cho [observables](https://github.com/zenparsing/es-observable). Nếu bạn trả về một observable từ một test, AVA sẽ tự động hoàn tất trước khi test kết thúc.

*Bạn không cần phải dùng "callback mode" hoặc gọi `t.end()`.*

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

### Hỗ trợ callback

AVA hỗ trợ sử dụng `t.end` như là final callback khi sử dụng node-style error-first callback APIs. AVA sẽ xem xét bất kì giá trị nào được truyền vào như là một đối số đầu tiên của `t.end` để nó trở thành một lỗi. Lưu ý rằng `t.end` cần "callback mode", thứ có thể được kích hoạt bằng cách dùng chuỗi `test.cb`.

```js
test.cb('data.txt can be read', t => {
	// `t.end` tự động xác nhận lỗi như là đối số đầu tiên
	fs.readFile('data.txt', t.end);
});
```

### Timeout toàn cục

Một timeout toàn cục có thể được đặt thông qua tùy chọn `--timeout`. Timeout trong AVA hoạt động khác với timeout trong các frameworks khác. AVA đặt lại một timer sau mỗi test, buộc test phải thoát nếu không có các kết quả của test mới nhận được trong thời gian timeout đã chỉ định. Điều này có thể được dùng để xử lý các test bị trì hoãn.

Bạn có thể đặt timeout theo cách:

```console
$ ava --timeout=10s # 10 seconds
$ ava --timeout=2m # 2 minutes
$ ava --timeout=100 # 100 milliseconds
```

### Chạy song song trong CI

AVA tự động phát hiện xem môi trường CI của bạn có hỗ trợ build song song hay không. Mỗi build sẽ chạy một tập con của tất cả các file test, trong khi vẫn đảm bảo tất cả các test được thực thi. Xem [`ci-parallel-vars`](https://www.npmjs.com/package/ci-parallel-vars) để biết danh sách các môi trường CI được hỗ trợ.

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

Tiêu đề của test.

#### `implementation(t)`

Type: `function`

Nên chứa một test thực tế.

##### `t`

Type: `object`

Đối tượng thực thi của một test cụ thể. Mỗi test nhận một đối tượng khác nhau. Chứa các [Xác nhận giá trị](#xác-nhận-giá-trị) cũng như các phương thức `.plan(count)` và `.end()`. `t.context()` có thể chứa trạng thái được chia sẻ từ các hook. `t.title` trả về tiêu đề của test.

###### `t.plan(count)`

Lên kế hoạch xem bao nhiêu xác nhận giá trị có trong test. Test sẽ thất bại nếu số lần xác nhận giá trị thực tế không trùng khớp với số lần xác nhận giá trị đã lên kế hoạch. Xem [Lập kế hoạch xác nhận giá trị](#lập-kế-hoạch-xác-nhận-giá-trị).

###### `t.end()`

Kết thúc test. Chỉ hoạt động với `test.cb()`.

###### `t.log(...values)`

Hiển thị các giá trị theo từng ngữ cảnh bên cạnh kết quả test thay vì ngay lập tức in chúng vào `stdout`. Hoạt động giống như `console.log`, nhưng không hỡ trợ placeholder tokens. 

## Xác nhận giá trị

Xác nhận giá trị được pha trộn vào [execution object](#t) được cung cấp cho mỗi lần thực hiện test:

```js
test('unicorns are truthy', t => {
	t.truthy('unicorn'); // Xác nhận giá trị
});
```

Các xác nhận giá trị được ràng buộc với test vì vậy bạn có thể gán chúng cho một biến hoặc chuyển chúng đi xung quanh:

```js
test('unicorns are truthy', t => {
	const truthy = t.truthy;
	truthy('unicorn');
});
```

Xác nhận giá trị có thể được bỏ qua bằng cách thêm `.skip()`:

```js
test('unicorns are truthy', t => {
	t.truthy.skip('unicorn');
});
```

Nếu gặp phải nhiều lỗi xác nhận giá trị trong một test, AVA sẽ chỉ hiển thị lỗi *đầu tiên*.

### `.pass([message])`

Vượt qua xác nhận giá trị.

### `.fail([message])`

Làm xác nhận giá trị thất bại.

### `.truthy(value, [message])`

Xác nhận rằng `value` là đúng đắn.

### `.falsy(value, [message])`

Xác nhận rằng `value` là không đúng đắn.

### `.true(value, [message])`

Xác nhận rằng `value` là `true`.

### `.false(value, [message])`

Xác nhận rằng `value` là `false`.

### `.is(value, expected, [message])`

Xác nhận rằng `value` là tương tự so với `expected`. Điều này dựa trên [`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is).

### `.not(value, expected, [message])`

Xác nhận rằng `value` là không tương tự như `expected`. Điều này dựa trên [`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is).

### `.deepEqual(value, expected, [message])`

Xác nhận rằng `value` là bằng chính xác (deeply equal) so với `expected`. Xem [Concordance](https://github.com/concordancejs/concordance) để biết thêm chi tiết. Hoạt động với [React elements and `react-test-renderer`](https://github.com/concordancejs/react).

### `.notDeepEqual(value, expected, [message])`

Xác nhận rằng `value` là không bằng chính xác so với `expected`. Ngược lại với `.deepEqual()`.

### `.throws(fn, [expected, [message]])`

Xác nhận rằng một lỗi đã được throw. `fn` phải là một hàm mà có throw lỗi. giá trị được throw *phải* là một lỗi. Nó được return do vậy bạn có thể chạy nhiều xác nhận giá trị hơn với nó.

Giá trị được throw *phải* là một lỗi. Nó được trả về để bạn có thể chạy nhiều xác nhận giá trị hơn.

`expected` có thể là một constructor, trong trường hợp đó lỗi được throw ra phải là một instance của constructor. Nó có thể là một chuỗi, được so sánh với thông báo lỗi được throw ra, hoặc một regular expression được đối sánh với thông báo này. Bạn cũng có thể chỉ định đối tượng đối sánh có một hoặc nhiều thuộc tính sau.

* `instanceOf`: một constructor, lỗi được throw ra phải là một instance của
* `is`: lỗi được throw ra phải đúng bằng `expected.is`
* `message`: có thể là một chuỗi, được so sánh với thông báo lỗi được throw ra, hoặc một regular expression được đối sánh với thông báo này
* `name`: giá trị `.name` của một lỗi được throw ra
* `code`: giá trị `.code` của một lỗi được throw ra

`expected` không cần phải được chỉ định. Nếu bạn không cần nó nhưng muốn thiết lập một thông báo xác nhận giá trị, bạn phải chỉ rõ `null`.

Ví dụ:

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

### `.throwsAsync(thrower, [expected, [message]]`

Xác nhận rằng một lỗi đã được throw. thrower có thể là một hàm không đồng bộ mà có throw lỗi, hoặc một promise phải reject. Xác nhận giá trị này phải được chờ.

Giá trị được throw *phải* là một lỗi. Nó được trả về để bạn có thể chạy nhiều xác nhận giá trị hơn.

`expected` có thể là một constructor, trong trường hợp đó lỗi được throw ra phải là một instance của constructor. Nó có thể là một chuỗi, được so sánh với thông báo lỗi được throw ra, hoặc một regular expression được đối sánh với thông báo này. Bạn cũng có thể chỉ định đối tượng đối sánh có một hoặc nhiều thuộc tính sau.

* `instanceOf`: một constructor, lỗi được throw ra phải là một instance của
* `is`: lỗi được throw ra phải đúng bằng `expected.is`
* `message`: có thể là một chuỗi, được so sánh với thông báo lỗi được throw ra, hoặc một regular expression được đối sánh với thông báo này
* `name`: giá trị `.name` của một lỗi được throw ra
* `code`: giá trị `.code` của một lỗi được throw ra

`expected` không cần phải được chỉ định. Nếu bạn không cần nó nhưng muốn thiết lập một thông báo xác nhận giá trị, bạn phải chỉ rõ `null`.

Ví dụ:

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

Khi test một promise, bạn phải chờ cho xác nhận giá trị được hoàn thành:

```js
test('rejects', async t => {
	await t.throws(promise);
});
```

Khi test một hàm bất đồng bộ (asynchronous function) bạn cũng phải chờ cho xác nhận giá trị được hoàn thành:

```js
test('throws', async t => {
	await t.throws(async () => {
		throw new TypeError('🦄');
	}, {instanceOf: TypeError, message: '🦄'});
});
```

### `.notThrows(nonThrower, [message])`

Xác nhận rằng không có lỗi được throw ra. `thrower` có thể là một hàm mà không nên được throw ra, hoặc trả về một promise có thể resolve. Hoặc một promise có thể hoàn tất thành công test một cách trực tiếp.

Giống như xác nhận `.throws()`, khi test một promise bạn phải chờ cho xác nhận giá trị được hoàn tất:

```js
test('resolves', async t => {
	await t.notThrows(promise);
});
```

### `.notThrowsAsync(nonThrower, [message])`

Xác nhận rằng không có lỗi được throw ra. `nonThrower` có thể là một hàm bất đồng bộ mà không nên throw lỗi, hoặc một promise cần phải resolve.

Giống như `.throwsAsync()`, bạn phải chờ cho xác nhận giá trị hoàn thành:

```js
test('resolves', async t => {
	await t.notThrowsAsync(promise);
});
```


### `.regex(contents, regex, [message])`

Xác nhận rằng `contents` trùng khớp với `regex`.

### `.notRegex(contents, regex, [message])`

Xác nhận rằng `contents` không trùng khớp với `regex`.

### `.snapshot(expected, [message])`
### `.snapshot(expected, [options], [message])`

So sánh giá trị `expected` với một snapshot đã được ghi lại trước đây. Các snapshot được lưu trữ cho mỗi test, vì vậy hãy đảm bảo bạn cho các test của mình những tiêu đề độc lập lẫn nhau. Ngoài ra, bạn có thể truyền một đối tượng `options` để chọn một snapshot cụ thể, ví dụ  `{id: 'my snapshot'}`.

Không thể bỏ qua các xác nhận snapshot khi các snapshot đang được cập nhật.

## Test với snapshot

AVA hỗ trợ test với snapshot, [như đã được giới thiệu bởi Jest](https://facebook.github.io/jest/docs/snapshot-testing.html), thông qua interface [Xác nhận giá trị](#xác-nhận-giá-trị). Bạn có thể ghi lại snapshot bất kì giá trị nào, cũng như các React element:

```js
// Component của bạn
const HelloWorld = () => <h1>Hello World...!</h1>;

export default HelloWorld;
```

```js
// Test của bạn
import test from 'ava';
import render from 'react-test-renderer';
import HelloWorld from '.';

test('HelloWorld component', t => {
	const tree = render.create(<HelloWorld/>).toJSON();
	t.snapshot(tree);
});
```

[Hãy thử nó trong các project ví dụ mẫu này](https://github.com/avajs/ava-snapshot-example)

Các snapshot được lưu trữ cùng với các file test. Nếu các test của bạn nằm trong thư mục `test` hoặc `tests`, các snapshot sẽ được lưu trữ trong thư mục `snapshot`. Nếu các test của bạn nằm trong thư mục `__tests__` thì các snapshot sẽ được lưu trữ trong thư mục `__snapshot__`.

Giả sử bạn có `~/project/test/main.js` chứa các snapshot xác nhận giá trị. AVA sẽ tạo 2 file:

* `~/project/test/snapshots/main.js.snap`
* `~/project/test/snapshots/main.js.md`

File đầu tiên chứa snapshot thực tế và cần cho các so sánh trong tương lai. File thứ hai chứa *báo cáo snapshot* của bạn. Chúng sẽ được tạo lại khi bạn cập nhật các snapshot của mình. Nếu bạn commit nó vào source của project, bạn có thể thấy được sự khác biệt của các thay đổi trong các snapshot của mình.

AVA sẽ hiển thị lý do tại sao xác nhận snapshot thất bại:

<img src="media/snapshot-testing.png" width="1048">

Sau đó, bạn có thể kiểm tra code của mình, Nếu thay đổi là cố ý, bạn có thể dùng cờ `--update-snapshots` (hoặc `-u`) để cập nhật các snapshot:

```console
$ ava --update-snapshots
```

Bạn có thể chỉ định một vị trí cố định để lưu các file snapshot trong [Cấu hình `package.json`](#cấu-hình) của AVA:

```json
{
	"ava": {
		"snapshotDir": "custom-directory"
	}
}
```

Các file snapshot được lưu trong cấu trúc thư mục sẽ phản ánh các file test của bạn.

Nếu bạn đang chạy AVA với các file test đã được compile trước, AVA sẽ cố thử và sử dụng source map để xác định vị trí của các file gốc. Snapshot sẽ được lưu bên cạnh các file này, tuân theo các quy tắc giống như khi AVA thực thi các file gốc một cách trực tiếp. Điều này rất tuyệt vời nếu bạn đang viết test của mình bằng TypeScript (Xem [Công thức TypeScript](docs/recipes/typescript.md) của chúng tôi).

### Bỏ qua các xác nhận giá trị

Bất kì các xác nhận giá trị đều có thể được bỏ qua bằng cách dùng `skip`. Các xác nhận giá trị đã bị bỏ qua vẫn sẽ được đếm, vì vậy bạn không cần phải thay đổi kế hoạch đếm các xác nhận giá trị của mình.

```js
test('skip assertion', t => {
	t.plan(2);
	t.is.skip(foo(), 5); // Không cần phải thay đổi kế hoạch đếm các xác nhận giá trị khi bỏ qua
	t.is(1, 1);
});
```

### Thông báo xác nhận giá trị nâng cao

AVA đi kèm với [`power-assert`](https://github.com/power-assert-js/power-assert) được tích hợp sẵn, cung cấp cho bạn nhiều thông báo xác nhận chi tiết tơn. Nó sẽ đọc test của bạn và cố suy ra nhiều thông tin hơn trong code.

Hãy lấy ví dụ này, sử dụng Node's standard [`assert` library](https://nodejs.org/api/assert.html):

```js
const a = /foo/;
const b = 'bar';
const c = 'baz';
require('assert').ok(a.test(b) || b === c);
```

Nếu bạn dán nó vào Node REPL nó sẽ trả về:

```
AssertionError: false == true
```

Tuy nhiên, trong AVA, test này:

```js
test('enhanced assertions', t => {
	const a = /foo/;
	const b = 'bar';
	const c = 'baz';
	t.true(a.test(b) || b === c);
});
```

Sẽ trả về:

```
t.true(a.test(b) || b === c)
       |      |     |     |
       |      "bar" "bar" "baz"
       false
```

## Cách ly process

Mỗi file test sẽ được chạy trong một Node.js process riêng biệt. Điều này cho phép bạn thay đổi trạng thái toàn cục hoặc ghi đè lên một trạng thái được tích hợp sẵn trong file test, mà không làm ảnh hưởng các file test khác. Nó cũng đem lại hiệu suất tuyệt vời trên các bộ xử lý nhiều core hiện đại, giúp nhiều file test được thực thi song song với nhau.

AVA sẽ đặt `process.env.NODE_ENV` vào  `test`, trừ khi biến môi trường `NODE_ENV`  chưa được thiết lập. Điều này rất hữu ích nếu code mà bạn đang test có các mặc định của test (ví dụ như chọn database nào để connect, hoặc tùy chọn môi trường cụ thể của Babel). Dù nó sẽ làm cho code của bạn và các dependencies của nó hành xử khác nhau. Lưu ý rằng `NODE_ENV` trong  process.env` sẽ luôn luôn là `true`.

## Các mẹo

### Các file tạm thời

Chạy các test đồng thời đem đến một số thách thức, làm file IO là một trường hợp.

Thông thường, các test nối tiếp tạo ra các thư mục tạm thời trong thư mục test hiện tại, và sẽ dọn dẹp chúng khi test kết thúc. Điều này sẽ không hoạt động khi bạn chạy các test đồng thời, bởi vì các test sẽ xung đột với nhau. Cách chính xác để làm điều này là sử dụng một thư mục tạm mới cho mỗi test. Các module [`tempfile`](https://github.com/sindresorhus/tempfile) và  [`temp-write`](https://github.com/sindresorhus/temp-write) có thể hữu ích với bạn.

### Code coverage

Bạn không thể dùng [`istanbul`](https://github.com/gotwarlost/istanbul) để kiểm tra code coverage như AVA [sinh ra các file test](#cách-ly-process). Thay vào đó bạn có thể dùng [`nyc`](https://github.com/bcoe/nyc), vì nyc về cơ bản là `istanbul` nhưng có sự hỗ trợ  subprocess.

Kể từ phiên bản `5.0.0`, AVA sẽ sử dụng các source map để báo cáo coverage cho code của bạn, bất kể sự transpile. Hãy đảm bảo rằng code mà bạn đang test có bao gồm inline source map hơạc tham chiếu đến một file source map. Nếu bạn dùng `babel-register` bạn có thể đặt tùy chọn `sourceMaps` trong cấu hình Babel của bạn thành `inline`.

### Những sai lầm phổ biến

Chúng tôi có một danh sách ngày càng tăng về [các sai lầm phổ biến](docs/common-pitfalls.md) mà bạn có thể gặp phải khi sử dụng AVA. Nếu bạn gặp phải bất kì vấn đề nào bạn nghĩ là phổ biến, hãy bình luận trong [issue này](https://github.com/avajs/ava/issues/404).

## FAQ

### Tại sao không phải là `mocha`, `tape`, `tap`?

Mocha yêu cầu bạn phải sử dụng các cấu hình toàn cục như `description` và `it` với interface mặc định (thứ mà hầu hết mọi người dùng). Điều đó không được đánh giá cao và việc thực thi các test theo thứ tự mà không cách ly process, sẽ làm cho test của bạn chạy chậm.

Tape và tap cũng khá là tốt. AVA được lấy cảm hứng từ cú pháp của chúng. Chúng cũng thực hiện các bài kiểm tra một cách tuần tự. Nhưng đầu ra mặc định [TAP](https://testanything.org) của chúng lại không thân thiện với người dùng, do vậy bạn luôn phải sử dụng các tap reporter khác.

Ngược lại, AVA được đánh giá cao và nó chạy các test một cách đồng thời, với một process độc lập cho mỗi file test. Reporter mặc định của AVA rất dễ đọc và không dừng lại ở đó AVA còn hỗ trợ TAP output thông qua cờ CLI.

### Tên project được viết và đọc như thế nào?

AVA, chứ không phải Ava hay ava. Được phát âm là [/ˈeɪvə/ ay-və](media/pronunciation.m4a?raw=true).

### Hình nền của tiêu đề là gì vậy?

Đó là [thiên hà Andromeda](https://simple.wikipedia.org/wiki/Andromeda_galaxy).

### Sự khác nhau giữa đồng thời và song song là gì?

[Đồng thời không phải là song song, nó cho phép song song.](https://stackoverflow.com/q/1050222)

## Các công thức

- [Cài đặt Test](docs/recipes/test-setup.md)
- [Code coverage](docs/recipes/code-coverage.md)
- [Chế độ theo dõi](docs/recipes/watch-mode.md)
- [Kiểm tra endpoint](docs/recipes/endpoint-testing.md)
- [Khi nào nên dùng `t.plan()`](docs/recipes/when-to-use-plan.md)
- [Test browser](docs/recipes/browser-testing.md)
- [TypeScript](docs/recipes/typescript.md)
- [Flow](docs/recipes/flow.md)
- [Cấu hình Babel][Babel recipe]
- [Sử dụng các module ES](docs/recipes/es-modules.md)
- [Truyền tham số vào các file test của bạn](docs/recipes/passing-arguments-to-your-test-files.md)
- [Test cho React components](docs/recipes/react.md)
- [Test cho Vue.js components](docs/recipes/vue.md)
- [JSPM và SystemJS](docs/recipes/jspm-systemjs.md)
- [Debug các test với Chrome DevTools](docs/recipes/debugging-with-chrome-devtools.md)
- [Debug các test với WebStorm](docs/recipes/debugging-with-webstorm.md)
- [Compile trước các file source với webpack](docs/recipes/precompiling-with-webpack.md)
- [Cách lý các test tích hợp của MongoDB](docs/recipes/isolated-mongodb-integration-tests.md)

## Hỗ trợ

- [Stack Overflow](https://stackoverflow.com/questions/tagged/ava)
- [Spectrum](https://spectrum.chat/ava)
- [Twitter](https://twitter.com/ava__js)

## Có liên quan

- [eslint-plugin-ava](https://github.com/avajs/eslint-plugin-ava) - Lint rules for AVA tests
- [sublime-ava](https://github.com/avajs/sublime-ava) - Snippets for AVA tests
- [atom-ava](https://github.com/avajs/atom-ava) - Snippets for AVA tests
- [vscode-ava](https://github.com/samverschueren/vscode-ava) - Snippets for AVA tests
- [gulp-ava](https://github.com/avajs/gulp-ava) - Run tests with gulp
- [grunt-ava](https://github.com/avajs/grunt-ava) - Run tests with grunt
- [More…](https://github.com/avajs/awesome-ava#packages)

## Các liên kết

- [AVA stickers, t-shirts, etc](https://www.redbubble.com/people/sindresorhus/works/30330590-ava-logo)
- [Awesome list](https://github.com/avajs/awesome-ava)
- [AVA Casts](http://avacasts.com)
- [More…](https://github.com/avajs/awesome-ava)

## Nhóm phát triển

[![Mark Wubben](https://github.com/novemberborn.png?size=100)](https://github.com/novemberborn) | [![Sindre Sorhus](https://github.com/sindresorhus.png?size=100)](https://github.com/sindresorhus) | [![Vadim Demedes](https://github.com/vadimdemedes.png?size=100)](https://github.com/vadimdemedes)
---|---|---
[Mark Wubben](https://novemberborn.net) | [Sindre Sorhus](http://sindresorhus.com) | [Vadim Demedes](https://github.com/vadimdemedes)

###### Những người phát triển trước đây

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
