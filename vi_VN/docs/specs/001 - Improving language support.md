# Cải thiện việc hỗ trợ ngôn ngữ

Các bản dịch: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/specs/001%20-%20Improving%20language%20support.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/specs/001%20-%20Improving%20language%20support.md)

Một [RFC](http://blog.npmjs.org/post/153881413635/some-notes-on-rfcs) với sự đề xuất cải thiện hỗ trợ của AVA cho các dự án Babel, React và Typescript

## Báo cáo vấn đề

Việc tích hợp AVA với các dự án dựa trên Babel là quá cồng kềnh. Người dùng cần phải cấu hình `babel-core / register` để các file trợ giúp hoặc source file được biên dịch. Cách cấu hình  các file test được biên dịch là khó hiểu. Các source file có thể [cần một cấu hình khác hoặc được load sẵn trong AVA][lý do tùy chọn source] so với khi build cho bản phân phối.

Không có hỗ trợ để viết các test và source trong các ngôn ngữ khác như TypeScript hoặc JSX.

## Lai lịch

AVA dùng Babel để cho phép người dùng viết các test sử dụng các đề xuất [ES2015](https://babeljs.io/docs/plugins/preset-es2015/) và [stage-2](https://babeljs.io/docs/plugins/preset-stage-2/). Thông báo xác nhận được [tăng cường bằng cách sử dụng một Babel plugin](https://github.com/avajs/ava/pull/46) và một plugin khác được dùng để [phát hiện việc sử dụng đúng và không đúng `t.throws()`](https://github.com/avajs/ava/pull/742).

Ban đầu `babel/register` được [dùng trực tiếp](https://github.com/avajs/ava/pull/23), và được áp dụng vào các file test, helper và source. Ngay sau đó điều này được thay đổi nên chỉ có [file test được transpile](https://github.com/avajs/ava/issues/50). Các hành vi trước đây đã [được coi như là một lỗi](https://github.com/avajs/ava/issues/108#issuecomment-151245367), có lẽ vì nó làm cho AVA biên dịch các file nguồn đi ngược lại với ý định của người dùng.

Các người dùng sau đó đã được [khuyên thêm `babel-core/register` vào danh sách các module tự động cần thiết khi chạy các test](https://github.com/avajs/ava#transpiling-imported-modules). Nhưng hóa ra việc load Babel trong mỗi process là khá chậm, và thay vào đó các nỗ lực đã được thực hiện để biên dịch các file [helper][1078] và [source][945] trong process chính của AVA.

Trong khi đó AVA đã chuyển từ sử dụng 'babel/register` sang sử dụng Babel trực tiếp. Một [triển khai cache](https://github.com/avajs/ava/pull/352) đã được xếp ở lớp trên cùng.

AVA chỉ tìm kiếm các file test với đuôi mở rộng `.js`, ngay cả khi các glob pattern được kết hợp rõ ràng với các file khác. Theo định nghĩa, điều này không ngăn cản việc các file [JSX và TypeScript được chọn](https://github.com/avajs/ava/issues/631).

## Các phương pháp khả thi

[#945][945] cố gắng để biên dịch tất cả các dependency của file test (cả các file helper và source) trong process chính. Ngoài các vấn đề chưa được giải quyết, một nhược điểm lớn là nó không thể xử lý các yêu cầu động, vì chúng xảy ra trong các worker process thay vì process chính. Việt quét các depencency đã làm tăng thêm chi phí cho chính nó.

[#1078][1078] biên dịch trước các file trong process chính, như cách các file test được biên dịch trước. Điều này sẽ hoạt động có lẽ là tốt, nhưng tất nhiên là sẽ có hạn chế về mặt biên dịch các file helper.

[Kết luận của #631][631 conclusion] là cho phép các phần mở rộng của file test khác nhau được chỉ định. Nhưng không may chỉ đơn thuần cho phép các phần mở rộng khác là không đủ. AVA sẽ vẫn giả sử rằng các file test chỉ chứa JavaScript. Nó sẽ không thể chạy JSX hoặc TypeScript.

[#1122](https://github.com/avajs/ava/pull/1122) xây dựng trên [đề xuất trong #631][631 conclusion] bằng cách phát hiện xem phần mở rộng `.ts` có được cấu hình là tự động biên dịch test đó hay không bằng cách sử dụng TypeScript. Thật không may là nó không rõ ràng làm thế nào để điều này hoạt động với các file source mà không cần chạy vào cùng các vấn đề về hiệu suất mà chúng tôi đã thấy với Babel. Các file test TypeScript  sẽ không nhận được các xác nhận nâng cao hoặc sự bảo vệ chống lại việc sử dụng `t.throws()` không đúng cách. Thật khó để truyền đạt điều này cho người dùng khi cách hỗ trợ TypeScript được kích hoạt là chỉ định tùy chọn của một `extension`.

## Đề xuất cụ thể

Theo mặc định, AVA sẽ biên dịch các file test và helper. Nó sử dụng Babel, nhưng chỉ với các plugin cho đề xuất stage-4 và các tiêu chuẩn được phê chuẩn (hiện tại đó là ES2016 cộng với các đề xuất đã đạt đến stage-4 và sẽ được đưa vào ES2017). Điều này có nghĩa là AVA hỗ trợ cú pháp giống như ESLint.

AVA đã không còn áp dụng [`babel-plugin-transform-runtime`](https://babeljs.io/docs/plugins/transform-runtime/). Plugin này tạo các alias ES2015 toàn cục, là điều không cần thiết, vì chúng ta đang nhắm đến mục tiêu Node.js 4. Đây là một [sai lầm đã biết](https://github.com/avajs/ava/issues/1089).

Các biến đổi khác của AVA, chẳng hạn như `babel-plugin-espower` và `babel-plugin-ava-throws-helper`, sẽ được đóng gói thành một preset `babel-preset-ava` và tự động được áp dụng. (Nếu cần, chúng tôi có thể thêm tùy chọn áp dụng `babel-plugin-transform-runtime` cùng với [rewrite logic](https://github.com/avajs/ava/blob/033d4dcdcbdadbf665c740ff450c2a775a8373dc/lib/babel-config.js#L53:L61) chúng tôi áp dụng để sửa chữa các đường dẫn (path). Chúng ta nên xem xét cách tiếp cận này.)

### Các dự án Babel

Ở trên giả định AVA được sử dụng với các dự án JavaScript thông thường mà không yêu cầu phải biên dịch. Nhiều người dùng mặc dù đã có một Babel pipeline tại chỗ và muốn sử dụng AVA mà không cần phải biên dịch trước các file source của họ.

Để đơn giản nhất, thiết lập `"babel": true` trong cấu hình AVA sẽ cho phép AVA hỗ trợ cho các dự án Babel. Các file test và helper sẽ được biên dịch theo như ở trên, nhưng các file source bây giờ đây cũng tự động được biên dịch.

AVA xem xét các file `package.json` hoặc `.babelrc` của các dự án cho các tùy chọn Babel sử dụng để biện dịch các file source (lý tưởng là chúng ta có thể trích xuất một đối tượng cấu hình Babel thích hợp từ hai vị trí này). Đây là cách đơn giản hóa quản lý cấu hình thực tế của Babel, tìm kiếm file tùy chọn gần nhất với tệp đang được biên dịch. Nhìn vào hai file cụ thể này cho phép AVA sử dụng kết quả biên dịch được lưu trong bộ nhớ cache mà không cần phải tải Babel, trong khi vẫn biên dịch lại các file source nếu các tùy chọn bị thay đổi.

Việc sử lý các dự án Babel của AVA có thể được cấu hình thêm bằng cách truyền một đối tượng tùy chọn thay vì `true`:

* `compileSources: true | false`: mặc định là `true`, xác định xem các source có được biên dịch hay không.
* `extensions: "js" | ["js", "jsx", ...]`: mặc định là `"js"`, chỉ định phần mở rộng của file được cho phép. Đều này mở rộng các pattern file test mặc định.
* `sourceOptions: {}`: chỉ định [Tùy chỉnh Babel] được sử dụng để biên dịch các file source. Trong bối cảnh này, `babelrc: true` làm cho các tùy chọn được hợp nhất với các tùy chọn được tìm thấy trong các file `package.json` hoặc `.babelrc` của dự án. `babelrc` mặc định là `true`.
* `testOptions: {}`: chỉ định [tùy chỉnh Babel] được sử dụng để biên dịch các file test và helper. Nếu được cung cấp, điều này sẽ vô hiệu hóa hoàn toàn cấu hình mặc định mà AVA sử dụng để biên dịch các file test và helper. Giống như `sourceOptions`, `babelrc` mặc định là `true`. Đặt `presets: ["ava"]` để áp dụng các biến đổi của AVA.

`sourceOptions` có thể được sử dụng để mở rộng cấu hình Babel được chia sẻ để các file source có thể được tải trong các test của AVA. Ví dụ người dùng có thể [dựa vào webpack để giải quyết cú pháp module ES2015 tại thời điểm build, nhưng vẫn cần áp dụng `babel-plugin-transform-es2015-modules-commonjs` cho source để làm việc trong AVA][lý do tùy chọn source].

`sourceOptions` và `testOptions`, là [các tùy chỉnh của Babel], có thể chỉ định các giá trị `ignore` và `only`. Chúng chỉ được sử dụng để xác định xem tệp có cần biên dịch hay không. Chúng không ảnh hưởng đến việc lựa chọn file test hoặc theo dõi source.

## Tài liệu sưu tập

Dựa trên [bằng chứng khái niêm](https://github.com/avajs/ava/pull/1082) này, việc biên dịch Babel được chuyển vào các test worker. Nếu như các file source được biên dịch, AVA sẽ tải hook cần thiết của riêng nó, thay vì dựa vào `babel-core/register`.

Tùy chọn Babel cho các file test, các file helper và source được chuẩn bị trong process chính, sau đó được chia sẻ với các worker. Các bộ nhớ đệm có nguồn gốc từ các cấu hình này cũng như các dependency khác có thể có liên quan.

Các worker băm nội dung file thô và kiểm tra bộ nhớ cache để xem liệu kết quả được biên dịch trước đó có thể được sử dụng hay không. (Do worker có thể chạy đồng thời, nên cẩn thận để đảm bảo rằng chúng đọc các mục trong bộ nhớ cache đầy đủ. Điều đó cũng được mặc dù nếu chỉ cùng một file được biên dịch nhiều lần.)

## Các dự án TypeScript

Sự hỗ trợ TypeScript có thể được cung cấp theo cách tương tự như hỗ trợ Babel nâng cao được mô tả ở trên. Thiết lập `"typescript": true` trong cấu hình AVA cho phép hỗ trợ TypeScript cho các file test `.ts` và helper, cũng như các source. Một đối tượng tùy chọn cũng có thể được cung cấp.

* `compileSources: true | false`: mặc định là `true`, xác định xem các source có được biên dịch hay không.
* `extensions: "ts" | ["ts", "tsx", ...]`: mặc định là `"ts"`, chỉ định phần mở rộng file được cho phép. Điều này mở rộng các pattern file test mặc định.
* `sourceOptions: {}`: chỉ định [tùy chọn TypeScript] được sử dụng để biên dịch các file source. Tùy chọn `extends` mặc định là file `tsconfig.json` của dự án, nếu có. Nó phải được đặt thành `null` để tránh mở rộng file này.
* `testOptions: {}`: chỉ định [tùy chọn TypeScript] được sử dụng để biên dịch các file test và helper. Hoạt động giống như `sourceOptions`, không có cấu hình mặc định cho các file test và helper, không giống như với các dự án Babel.

Đối với `sourceOptions` và `testOptions`, là [tùy chọn TypeScript], `files`, `include` và `exclude` không ảnh hưởng đến việc lựa chọn file test hoặc theo dõi source.

## Các chi tiết triển khai khác

Cả sự hỗ trợ Babel và TypeScript đều có thể được cung cấp thông qua các module Node.js riêng biệt. Chúng nên implement cùng interface, để làm cho việc tích hợp với AVA dễ dàng hơn.

AVA có hỗ trợ Babel, tuy nhiên cần phải cài đặt một dependency riêng biệt để hỗ trợ sự hoạt động của TypeScript. Một lỗi hữu ích được ghi lại nếu dependency này bị thiếu khi hỗ trợ TypeScript được bật.

AVA chọn các tệp thử nghiệm dựa trên cấu hình `babel` và `typescript` được kết hợp.

Các đường dẫn tương đối trong `sourceOptions` và `testOptions` [phải được giải quyết đường dẫn tương đối liên quan đến file `package.json`](https://github.com/avajs/ava/issues/707).

[1078]: https://github.com/avajs/ava/pull/1078
[631 conclusion]: https://github.com/avajs/ava/issues/631#issuecomment-248659780
[945]: https://github.com/avajs/ava/pull/945
[Babel options]: https://babeljs.io/docs/usage/api/#options
[source options reason]: https://github.com/avajs/ava/issues/1139#issuecomment-267969417
[TypeScript options]: https://www.typescriptlang.org/docs/handbook/tsconfig-json.html
