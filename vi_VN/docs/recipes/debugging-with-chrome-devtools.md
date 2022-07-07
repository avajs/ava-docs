# Gỡ lỗi cho các test với Chrome DevTools

Các bản dịch: [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/debugging-with-chrome-devtools.md), [Tiếng việt](https://github.com/avajs/ava-docs/blob/master/vi_VN/docs/recipes/debugging-with-chrome-devtools.md)

Dùng [inspect-process](https://github.com/jaridmargolin/inspect-process) để có thể dễ dàng khởi chạy một session gỡ lỗi với  Chrome DevTools.

```console
$ npm install --global inspect-process
```

```console
$ inspect node_modules/ava/profile.js some/test/file.js
```
