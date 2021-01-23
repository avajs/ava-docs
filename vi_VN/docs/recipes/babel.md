# Cấu hình babel

Các bản dịch: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/babel.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/recipes/babel.md)

AVA sử dụng [Babel 7](https://babeljs.io) để bạn có thể sử dụng cú pháp JavaScript mới nhất trong các test của mình. Chúng tôi thực hiện việc này bằng cách biên dịch các file test và helper bằng việc sử dụng preset [`@ava/stage-4`](https://github.com/avajs/babel-preset-stage-4) của chúng tôi. Chúng tôi cũng sử dụng [preset thứ 2, `@ava/transform-test-files`](https://github.com/avajs/babel-preset-transform-test-files) để bật [thông báo xác nhận nâng cao](../../readme#enhanced-assertion-messages) và phát hiện việc sử dụng các xác nhận `t.throws()` không đúng cách.

Theo mặc định, Babel pipeline của chúng tôi được áp dụng vào các file test và helper có đuôi mở rộng là `.js`. Nếu dự án của bạn sử dụng Babel thì chúng tôi sẽ tự động biên dịch các tệp này bằng cấu hình Babel trong dự án của bạn. Cài đặt trước preset `@ava/transform-helper-files` được áp dụng trước và cuối cùng là `@ava/stage-4`.

Nếu bạn đang sử dụng Babel cho các file source của bạn thì bạn cũng phải [cấu hình biên dịch source](#compile-sources).

## Tùy chỉnh các AVA biên dịch các file test của bạn

Bạn có thể ghi đè lên cấu hình Babel mặc định mà AVA sử dụng để biên dịch file test trong `package.json` hoặc `ava.config.js`. Ví dụ: cấu hình bên dưới sẽ thêm hỗ trợ cho cú pháp JSX và các tính năng của stage 3:

```json
{
	"ava": {
		"babel": {
			"testOptions": {
				"plugins": ["@babel/plugin-syntax-jsx"],
				"presets": ["@babel/preset-stage-3"]
			}
		}
	}
}
```

Tất cả các tùy chọn `.babelrc` đều được cho phép bên trong đối tượng `testOptions`.

## Đặt lại bộ nhớ đệm của AVA

AVA lưu trữ các file test và helper đã biên dịch. Nó sẽ tự động biên dịch lại các tệp này khi bạn thay đổi chúng, tuy nhiên nó không thể phát hiện các bản cập nhật của các plugin và cài đặt trước Babel của bạn hoặc các thay đổi đối với file cấu hình Babel của bạn.

Thay vào đó hãy chạy lệnh dưới đây để reset lại cache của AVA khi bạn thay đổi cấu hình hoặc cập nhật các plugin hay preset:

```console
$ npx ava --reset-cache
```

## Thêm các tiện ích bổ sung

Bạn có thể định cấu hình của AVA để nhận ra các phần mở rộng file bổ sung và biên dịch các file test và helper đó bằng Babel:

```json
{
	"ava": {
		"babel": {
			"extensions": [
				"js",
				"jsx"
			]
		}
	}
}
```

Xem thêm [tùy chọn `mở rộng`](../../readme.md#options) của AVA.

## Làm cho AVA bỏ qua tùy chỉnh Babel trong dự án của bạn

Bạn có thể không muốn AVA sử dụng các tùy chọn Babel trong dự án của bạn, ví dụ nếu dự án của bạn dựa vào Babel 6. Bạn có thể đặt tùy chọn `babelrc` thành `false`:

```json
{
	"ava": {
		"babel": {
			"testOptions": {
				"babelrc": false
			}
		}
	}
}
```

## Vô hiệu hóa stage-4 preset của AVA

Bạn có thể tắt stage-4 preset của AVA:

```json
{
	"ava": {
		"babel": {
			"testOptions": {
				"presets": [
					["module:ava/stage-4", false]
				]
			}
		}
	}
}
```

Lưu ý rằng điều này *không* ngăn AVA biên dịch các file test của bạn bằng Babel.

Bạn **phải** vô hiệu hóa preset bằng cách cấu hình nó trong `testOptions`. AVA sẽ vẫn áp dụng giá trị đặt sẵn nếu bạn định cấu hình nó trong các tệp khác (ví dụ: tệp `babelrc`). Đây là [do vấn đề của Babel](https://github.com/babel/babel/issues/7920).

## Duy trì cú pháp module ES

Theo mặc định preset stage-4 của AVA sẽ chuyển đối cú pháp của module ES thành CommonJS. Bạn có thể vô hiệu hóa điều này:

```json
{
	"ava": {
		"babel": {
			"testOptions": {
				"presets": [
					["module:ava/stage-4", {"modules": false}]
				]
			}
		}
	}
}
```

Bạn **phải** định cấu hình preset trong `testOptions` để giữ nguyên cú pháp module ES. AVA vẫn sẽ áp dụng preset nếu bạn cấu hình nó trong các file khác (ví dụ: file `.babelrc`). Điều này là [do vấn đề của Babel](https://github.com/babel/babel/issues/7920).

Bạn sẽ phải sử dụng module [`esm`](https://github.com/standard-things/esm) để AVA vẫn có thể tải các file test của bạn. [Xem công thức của chúng tôi để biết thêm chi tiết](./es-modules.md).

## Vô hiệu hóa Babel pipeline của AVA

Bạn hoàn toàn có thể vô hiệu hóa việc sử dụng Babel của AVA:

```json
{
	"ava": {
		"babel": false,
		"compileEnhancements": false
	}
}
```

## Sử dụng Babel polyfills

AVA cho phép bạn viết các test của mình bằng cách sử dụng cú pháp JavaScript mới, ngay cả trên các phiên bản Node.js không hỗ trợ nó. Tuy nhiên, nó sẽ không thêm hoặc sửa đổi sự tích hợp môi trường hiện tại của bạn. Ví dụ, việc sử dụng AVA sẽ không cung cấp các tính năng hiện đại như `Object.entries()` cho môi trường Node.js 6 trở xuống.

Bằng cách load [module `polyfill` của Babel](https://babeljs.io/docs/usage/polyfill/) bạn có thể chọn sử dụng các tính năng này. Lưu ý rằng điều này sẽ sửa đổi môi trường, điều này có thể ảnh hưởng đến cách chương trình của bạn hoạt động.

Bạn có thể kích hoạt module `polyfill` bằng cách thêm nó vào tùy chọn `require` của AVA:

```json
{
	"ava": {
		"require": [
			"@babel/polyfill"
		]
	}
}
```

Bạn sẽ cần phải tự cài đặt `@babel/polyfill`.

## Biên dịch mã nguồn

AVA hiện không biên dịch các file source. Bạn sẽ cần phải load [module `register` của Babel](http://babeljs.io/docs/usage/require/), thứ sẽ giúp biên dịch các file source nếu cần.

Bạn có thể kích hoạt `register` bằng cách thêm nó vào tùy chọn `require` của AVA:

```json
{
	"ava": {
		"require": [
			"@babel/register"
		]
	}
}
```

Bạn sẽ cần phải tự cài đặt `@babel/register`.

`@babel/register` cũng *sẽ* xử lý các file test và helper. Đối với hầu hết các trường hợp sử dụng, điều này là không cần thiết. Nếu bạn tạo một file với yêu cầu `@babel/register` bạn có thể cho nó biết đường dẫn file nào cần phải bỏ qua. Ví dụ trong thư mục `test` của bạn tạo `_register.js`:

```js
// test/_register.js:
require('@babel/register')({
	// Các pattern này liên quan đến thư mục dự án (nơi file `package.json` tồn tại):
	ignore: ['test/*']
});
```

Bây giờ thay vì yêu cầu `@babel/register`, hãy yêu cầu `test/_register`:

```json
{
	"ava": {
		"require": [
			"test/_register.js"
		]
	}
}
```

Lưu ý rằng việc load `@babel/register` trong mỗi worker process có ảnh hưởng hiệu năng không nhỏ. Nếu bạn có nhiều file test, bạn có thể sẽ muốn xem xét sử dụng một build step để biên dịch các nguồn của bạn. Đây không phải là điều lý tưởng, vì nó phức tạp khi sử dụng chế độ theo dõi của AVA, vì vậy chúng tôi khuyên bạn nên dùng `@babel/register` cho đến khi chi phí cho hiệu năng trở nên quá lớn. Thiết lập một bước biên dịch trước nằm ngoài phạm vi của tài liệu này, nhưng chúng tôi khuyên bạn nên kiểm tra một trong các [cách xây dựng hệ thống hỗ trợ Babel](http://babeljs.io/docs/setup/). Đây là [một vấn đề](https://github.com/avajs/ava/issues/577) đang được thảo luận về các cách mà chúng tôi có thể làm cho trải nghiệm này được tốt hơn.
