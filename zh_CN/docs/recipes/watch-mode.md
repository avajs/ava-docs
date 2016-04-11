___
**备注**

这是 [watch-mode.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/watch-mode.md) 的简体中文翻译。这个[链接](https://github.com/sindresorhus/ava/compare/82c02bce80696547db0387dec243ddb470c8bce7...master#diff-92da4f3d087d796fdf4a45be88586b62) 用来查看本翻译与 AVA 的 master 分支是否有差别（如果你没有看到`watch-mode.md`发生变化，那就意味着这份翻译文档是最新的）。
___

# 观察模式

翻译：[Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/watch-mode.md), [Italiano](https://github.com/sindresorhus/ava-docs/blob/master/it_IT/docs/recipes/watch-mode.md), [Русский](https://github.com/sindresorhus/ava-docs/blob/master/ru_RU/docs/recipes/watch-mode.md), [简体中文](https://github.com/sindresorhus/ava-docs/blob/master/zh_CN/docs/recipes/watch-mode.md)


AVA 自带了一个聪明的观察模式，它会观察那些改变了的文件并运行受到改变影响的测试。

## 运行测试时启用观察模式

你可以通过使用`--watch`或`-w`标志来启用观察模式，如果你是全局安装的 AVA：

```console
$ ava --watch
```

如果你将 AVA 配置在`package.json`中，像这样：

```json
{
  "scripts": {
    "test": "ava"
  }
}
```

你可以这样运行：

```console
$ npm test -- --watch
```

你可以设置一个特殊的脚本：

```json
{
  "scripts": {
    "test": "ava",
    "test:watch": "ava --watch"
  }
}
```

然后使用：

```console
$ npm run test:watch
```

## 要求

AVA 使用 [`chokidar`] 来作为文件观察器，它被配置为可选的依赖库，因为`chokidar`有时候无法安装，如果`chokidar`安装失败那么观察模式就不可用，然后你将看到一条这样的信息：

> The optional dependency chokidar failed to install and is required for --watch. Chokidar is likely not supported on your platform.

请参考 [`chokidar`文档][`chokidar`] 了解如何解决这个问题。

## 源文件和测试文件

在 AVA 中*源文件*和*测试文件*是有差别的，正如你所想的一样，*测试文件*包含了你的测试，*源文件*是需要支持测试运行的其他所有文件，是你的源代码或者测试数据。

默认情况下 AVA 观察测试文件，`package.json`和其他的`.js`文件的改变，它会忽略由 [`ignore-by-default`] 包提供的[特定文件夹](https://github.com/novemberborn/ignore-by-default/blob/master/index.js) 下的文件。

你可以使用 [`--source` CLI 标志]或`package.json`文件的`ava`属性为源文件配置模式，注意如果你从 [`ignore-by-default`] 中指定了一个负模式目录，那么忽略将不再有效，所以你可能想要在你的配置里重复这些操作。

如果你的测试会写入磁盘，那么它们会跟踪观察器来返回你的测试，这种情况下你需要使用`--source`标志。

## 依赖跟踪

AVA 跟踪测试文件依赖的源文件，如果你改变的源文件只有一个测试被依赖，那么就只会返回这个测试，如果它不能识别哪个测试文件依赖了这个被修改的源文件，那么它会返回所有的测试。

依赖跟踪在 required 模式中有效，支持自定义继承和转换，使用 [`--require` CLI 标志]而不是从你的测试文件来帮助你加载它们。使用`fs`模块来访问的文件不会被跟踪。

## 手动返回所有测试

你可以在 console 中，通过在<kbd>Enter</kbd>后面打印<kbd>r</kbd>来快速返回所有测试，

## 调试

有时候观察模式会出现一些意想不到的事情，比如当你只运行一个测试时会返回所有测试，为了调查原因你可以启用调试模式：

```console
$ DEBUG=ava:watcher npm test -- --watch
```

在 Windows 里这样用：

```console
$ set DEBUG=ava:watcher
$ npm test -- --watch
```

## 帮助我们改善观察模式

观察模式比较新并且现在处于初期阶段，请[报告](https://github.com/sindresorhus/ava/issues) 任何你遇到问题，谢谢！

[`chokidar`]: https://github.com/paulmillr/chokidar
[`ignore-by-default`]: https://github.com/novemberborn/ignore-by-default
[`--require` CLI 标志]: https://github.com/sindresorhus/ava-docs/blob/master/zh_CN/readme.md#cli
[`--source` CLI 标志]: https://github.com/sindresorhus/ava-docs/blob/master/zh_CN/readme.md#cli
