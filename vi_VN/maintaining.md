# Duy trì dự án [![Dependency Status](https://david-dm.org/avajs/ava.svg)](https://david-dm.org/avajs/ava) [![devDependency Status](https://david-dm.org/avajs/ava/dev-status.svg)](https://david-dm.org/avajs/ava#info=devDependencies)


## Cách hành xử

**Hãy tử tế với tất cả mọi người.**
Đọc và tuân thủ [Quy tắc ứng xử](code-of-conduct.md).


## Kiểm lỗi

 - `npm test`: Kiểm tra lỗi code và chạy toàn bộ các bộ kiểm lỗi cho toàn bộ các chức năng.
 - `npm run test-win`: Chạy các bộ kiểm lỗi trên Windows.
 - `tap test/fork.js --bail`: Chạy một bộ kiểm lỗi cụ thể và ngưng ngay khi gặp lỗi (hữu ích khi tìm lỗi).


## Quy trình xuất bản 

- Cập nhật dependencies.
- Đảm bảo [Travis CI](https://travis-ci.org/avajs/ava) và [AppVeyor](https://ci.appveyor.com/project/avajs/ava/branch/master) đều màu xanh lá (chạy thành công).
- Cập nhật [nếu cần thiết](docs/support-statement.md) trường `engines` trong `package.json`
- Xuất bản một phiên bản mới, dùng [`np`](https://github.com/sindresorhus/np) với số phiên bản tuân theo [semver](http://semver.org).
- Viết một [ghi chú xuất bản](https://github.com/avajs/ava/releases/new) theo cách trình bày của các ghi chú xuất bản trước đó.


## Các pull request

- Các tính năng mới nên đi kèm với các bộ kiểm tra và tài liệu.
- Đảm bảo đã làm theo [hướng dẫn đóng góp](contributing.md).
- Phải có ít nhất một thành viên trong nhóm đánh dấu `LGTM` cho một pull request trước khi nó được merge.
- Tổng hợp các commit lại khi merge. *[Ví dụ](https://github.com/avajs/ava/commit/0675d3444da6958b54c7e5eada91034e516bc97c)*


## Các nhãn issue

Thêm các nhãn sau để phân loại các issue:

* `babel`: Sử dụng nhãn này khi issue có liên quan đến cơ sở hạ tầng Babel của chúng tôi
* `blocked`: Sử dụng nhãn này khi issue đang bị cản trở . Vui lòng để lại nhận xét hoặc chỉnh sửa mô tả issue với vấn đề hiện đang làm issue bị cản trở.
* `bug`: Sử dụng nhãn này cho các lỗi của AVA
* `DO NOT MERGE`: Sử dụng nhãn này cho các pull request mang tính nghiên cứu và không nên merge
* `docs`: Sử dụng nhãn này để theo dõi các cải tiến cho tài liệu
* `enhancement`: Sử dụng nhãn này cho các yêu cầu tính năng mới
* `good for beginner`: Sử dụng nhãn này cho các issue hữu ích với những người mới sử dụng
* `help wanted`: Sử dụng nhãn này cho các issue mà chúng tôi thực sự mong được sự giúp đỡ từ những người khác không trong nhóm cốt lõi.
* `performance`: Sử dụng nhãn này cho các issue liên quan đến hiệu suất
* `question`: Sử dụng nhãn này cho các vấn đề đang trong giai đoạn thảo luận

Xin hãy chú ý các nhãn thể hiện sự ưu tiên:

* `priority`: Các issue cần giải quyết càng sớm càng tốt
* `low priority`: Các issue mà chúng tôi mong có sự tiến triển
* `future`: Các issue mà chúng tôi sẽ không lên kế hoạch để thực hiện trong thời gian gần. Đây là những đề xuất dài hạn mà chúng tôi không thể chấp nhận PRs

Sử dụng nhãn `assigned` khi có ai đó đang làm việc trên issue để chúng ta có thể tránh các nỗ lực trùng lặp.

## Giám sát

Đầu tiên bạn cần cài đặt [`iron-node`](https://github.com/s-a/iron-node) và / hoặc [`devtool`](https://github.com/Jam3/devtool) toàn cục:

```
$ npm install --global iron-node devtool
```

Trong thư mục gốc của một dự án sử dụng AVA, thực thi:

```
$ iron-node node_modules/ava/profile.js <test-file>
```

Hoặc:

```
$ devtool node_modules/ava/profile.js <test-file>
```

Khi cửa sổ Dev Tools đã được tải xong, kích hoạt giám sát Memory hoặc CPU, sau đó nhấn <kbd>Cmd</kbd> <kbd>R</kbd> để chạy lại các kiểm tra.

Ngay khi các bài kiểm tra kết thúc, hãy ngừng recording và kiểm tra các kết quả giám sát. Biểu đồ flamegraph có thể được hiển thị bằng cách chọn `Chart` từ trình đơn thả xuống trên tab `Profile` (Các view khác bao gồm `Tree (từ trên xuống)` và `Heavy (từ dưới lên)`).

Có thể bạn cũng muốn xem qua trang Settings của Dev Tools và kích hoạt một hoặc nhiều tùy chọn trong phần Profiling.

##### Các nguồn tài nguyên hữu ích

 - [Hướng dẫn debug Node.js với `devtool`](http://mattdesl.svbtle.com/debugging-nodejs-in-chrome-devtools).
 - [Video hướng dẫn về việc giám sát CPU và Memory bằng Chrome DevTools](https://www.youtube.com/watch?v=KKwmdTByxLk).


## Chấm điểm Benchmark

Đầu tiên thu thập dữ liệu benchmark của một nhánh/commit:

```
$ node bench/run
```

Khi bạn đã thu thập được dữ liệu từ 2,3 nhánh/ commit:

```
$ node bench/compare
```

*Chẳng hạn, bạn có thể thu thập dữ liệu benchmark từ working tree và commit cuối cùng.*

![](https://cloud.githubusercontent.com/assets/4082216/12700805/bf18f730-c7bf-11e5-8a4f-fec0993c053f.png)

Bây giờ bạn có thể khởi chạy một tập hợp con của bộ benchmark:

```
$ node bench/run.js concurrent/sync.js serial/sync.js -- concurrent/sync.js -- serial/sync.js
```

Lưu ý dấu `--`. Lệnh bên trên tương đương với việc chấm điểm benchmark cho tất cả các lệnh dưới đây.

```
$ ava concurrent/sync.js serial/sync.js
$ ava concurrent/sync.js
$ ava serial/sync.js
```

Ngoài ra nếu bạn đang chấm điểm benchmark một bộ mà có thể thất bại, bạn nên thêm cờ `--should-fail` vào bộ đó.

```
$ node bench/run.js concurrent/sync.js -- --should-fail other/failures.js
```

Lệnh trên tiến hành chấm điểm benchmark cho 2 lệnh, nhưng dự kiến lệnh thứ 2 sẽ thất bại


## Trở thành thành viên cốt lõi

- Thêm user vào `readme.md` và `package.json`.
- Thên user dưới dạng một cộng tác viên cho tất cả các repos và npm package có liên quan đến AVA.
- Chia sẻ thông tin về tài khoản Twitter và khuyến kích tweet/retweet những thứ có liên quan.
