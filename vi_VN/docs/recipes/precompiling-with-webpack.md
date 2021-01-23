## Biên dịch trước các file source với webpack

Các bản dịch: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/precompiling-with-webpack.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/recipes/precompiling-with-webpack.md)

AVA [readme](https://github.com/avajs/ava#transpiling-imported-modules) đề cập đến việc biên dịch trước các module đã import của bạn như là một sự thay thế cho trình biên dịch runtime, nhưng điều đó không được giải thích là phải làm thế nào. Công thức này thảo luận một số cách tiếp cận bằng cách sử dụng webpack v2. Nhiều cách tiếp cận được thảo luận vì mỗi phương pháp đều có ưu và nhược điểm riêng. Bạn nên chọn phương pháp phù hợp nhất với trường hợp sử dụng của mình. Xem  các cuộc thảo luận ban đầu [tại đây](https://github.com/avajs/ava/pull/1385).

- [Test một file](#test-một-file)
- [Test nhiều file](#test-nhiều-file)

### Test một file

Đây là trường hợp sử dụng đơn giản nhất. Bạn có thể cần điều này nếu bạn đang [sử dụng alias](https://github.com/avajs/ava/issues/1011).

###### `webpack.config.js`

```js
const path = require('path');
const nodeExternals = require('webpack-node-externals');

module.exports = {
	entry: ['test.js'],
	target: 'node',
	output: {
		path: path.resolve(__dirname, '_build'),
		filename: 'test.js'
	},
	externals: [nodeExternals()],
	module: {
		rules: [
			{
				test: /\.(js|jsx)$/,
				use: 'babel-loader',
				options: {
					cacheDirectory: true
				}
			}
		]
	}
};
```

Các bit quan trọng là `target: 'node'`, nó sẽ bỏ qua các require `require`s (e.g. `fs`, `path`, v.v.) cụ thể của Node.js và `externals: nodeModules` để ngăn chặn webpack không cố gắng đóng gói các module bên ngoài Node.js có thể bị ngắt.

Bây giờ bạn có thể chạy `$ npx ava _build/test.js`  để chạy các test chứa trong output này.

### Test nhiều file

Mọi thứ phức tạp hơn một chút với nhiều file test. Chúng tôi khuyên bạn nên [sử dụng `@babel/register`](babel.md##compile-sources) cho đến khi chi phí hiệu năng trở nên quá lớn.

Các cách tiếp cận có thể là:

- [Tham khảo biên dịch source trong test](#tham-khảo-biên-dịch-source-trong-test)
- [File entry duy nhất](#file-entry-duy-nhất)
- [Nhiều file entry](#nhiều-file-entry)
- [Test lại các nguồn đã biên dịch](#test-lại-các-nguồn-đã-biên-dịch)

#### Tham khảo biên dịch source trong test

Các file source có thể được biên dịch vào thư mục `_src` và được tham chiếu trong các test. Dù điều này không ổn lắm, nhưng nó thực hiện tốt và công việc có thể được tối ưu hóa bởi [chế độ theo dõi `babel-cli`](https://babeljs.io/docs/usage/cli/#babel).

```js
// Trước
import fresh from '../src';
// Sau
import fresh from '../_src';
```

#### File entry duy nhất

Nhiều file test có thể được biên dịch thành một file duy nhất. Điều này có thể có hiệu suất tốt nhất, nhưng nó có chi phí. Tất cả các bài kiểm tra sẽ nằm trong cùng một file, điều này có thể khiến bạn khó biết được test nào thất bại, vì AVA không hiển thị tên file của test. Bạn cũng sẽ mất [process cô lập](https://github.com/avajs/ava#process-isolation).

###### `webpack.config.js`

[Câu trả lời liên quan trên Stack Overflow](http://stackoverflow.com/questions/32874025/how-to-add-wildcard-mapping-in-entry-of-webpack/34545812#34545812)

```js
const path = require('path');
const glob = require('glob');
const nodeExternals = require('webpack-node-externals');

module.exports = {
	target: 'node',
	entry: glob.sync('./test/**/*.js'),
	output: {
		path: path.resolve(__dirname, '_build'),
		filename: 'tests.js'
	},
	externals: [nodeExternals()],
	module: {
		rules: [
			{
				test: /\.(js|jsx)$/,
				use: {
					loader: 'babel-loader',
					options: {
						cacheDirectory: true
					}
				}
			}
		]
	}
};
```

<details>
<summary>Error report comparison</summary>

```
# Before
  aggregations-test » cardinality-agg » sets precision_threshold option
  E:\Projects\repos\elastic-builder\test\_macros.js:167

   166:         const expected = getExpected(keyName, recursiveToJSON(propValue));
   167:         t.deepEqual(value, expected);
   168:     }

  Difference:

      Object {
        my_agg: Object {
          cardinality: Object {
    -       precision_threshol: 5000,
    +       precision_threshold: 5000,
          },
        },
      }

# After
  sets precision_threshold option
  E:\Projects\repos\elastic-builder\_build\tests.js:106

   105:                     column: 21
   106:                 }
   107:             },

  Difference:

      Object {
        my_agg: Object {
          cardinality: Object {
    -       precision_threshol: 5000,
    +       precision_threshold: 5000,
          },
        },
      }

```
</details>

#### Nhiều file entry

Chúng tôi có thể yêu cầu webpack tạo nhiều file entry. Điều này giúp giữ lại tên file để báo cáo lỗi dễ hiểu hơn. Nhưng mỗi file entry đều có bản sao riêng của file nguồn. Điều này dẫn đến kích thước tệp lớn hơn đáng kể. Điều này có thể [có hiệu năng khá kém](https://github.com/avajs/ava/pull/1385#issuecomment-304684047) trong lần thực thi đầu tiên.

###### `webpack.config.js`

```js
const path = require('path');
const glob = require('glob');
const nodeExternals = require('webpack-node-externals');

const entryObj = glob.sync('./test/**/*.js')
	.reduce((acc, file) => {
		acc[path.basename(file, path.extname(file))] = file;
		return acc;
	}, {});

module.exports = {
	target: 'node',
	entry: entryObj,
	output: {
		path: path.resolve(__dirname, '_build'),
		filename: '[name].js'
	},
	externals: [nodeExternals()],
	module: {
		rules: [
			{
				test: /\.(js|jsx)$/,
				use: {
					loader: 'babel-loader',
					options: {
						cacheDirectory: true
					}
				}
			}
		]
	}
};
```

#### Test lại các nguồn đã biên dịch

Đây là cách phức tạp nhất để thiết lập nhưng có hiệu năng khá tốt và cũng giữ lại tên file. Trong phương pháp này, chúng tôi sử dụng `babel-cli` để biên dịch các file nguồn nhưng vẫn giữ nguyên cấu trúc file. Yêu cầu đường dẫn trong các test được viết lại khi biên dịch chúng trong webpack. Ví dụ sau dành cho cấu trúc file thử nghiệm. Tùy thuộc vào cách file source và test được sắp xếp, bạn có thể cần thực hiện các thay đổi.

Cấu trúc thư mục:

```
├───src
│   ├───my-pkg-fldr
│   │   ├───my-module.js
│   │   └───index.js
│   └───index.js
└───test
    ├───my-pkg-fldr
    │   └───my-module.test.js
    └───index.test.js

# Generated file structure
├───_src
│   ├───my-pkg-fldr
│   │   ├───my-module.js
│   │   └───index.js
│   └───index.js
└───_build
    ├───my-module.test.js
    └───index.test.js
```

npm scripts:

```js
{
	"scripts": {
		"precompile-src": "cross-env NODE_ENV=test babel src --out-dir _src",
		"precompile-tests": "cross-env NODE_ENV=test webpack --config webpack.config.js",
		"pretest": "npm run precompile-src && npm run precompile-tests",
		"test": "cross-env NODE_ENV=test nyc --cache ava _build"
	}
}
```

###### `webpack.config.js`

[Các tài liệu `khác` của webpacks](https://webpack.js.org/configuration/externals/#function)

```js
const path = require('path');
const glob = require('glob');
const nodeExternals = require('webpack-node-externals');

const entryObj = glob.sync('./test/**/*.js')
	.reduce((acc, file) => {
		acc[path.basename(file, path.extname(file))] = file;
		return acc;
	}, {});

module.exports = {
	target: 'node',
	entry: entryObj,
	output: {
		path: path.resolve(__dirname, '_build'),
		filename: '[name].js'
	},
	externals: [
		nodeExternals(),
		// Viết lại đường dẫn yêu cầu để sử dụng `_src`
		(context, request, callback) => {
			// Đây là một chút lộn xộn vì các test không được xuất ra trong cấu trúc thư mục gốc
			// test/index.test.js → _build/index.test.js
				//=> ../src → ../_src
			// test/my-pkg-fldr/my-module.test.js → _build/my-module.test.js
				//=> ../../src → ../_src
			if (request.includes('/src')) {
				const requestReqwrite = request
					.replace('/src', '/_src')
					.replace('../../_src', '../_src');
				return callback(null, `commonjs ${requestReqwrite}`);
			}

			callback();
		}
	]
};
```
