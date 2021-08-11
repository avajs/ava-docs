___
**备注**

这是 [code-coverage.md](https://github.com/avajs/ava/blob/main/docs/recipes/code-coverage.md) 的简体中文翻译。这个[链接](https://github.com/avajs/ava/compare/1868204c1901f45b4f66a520ef6486fdd71fe1d2...master#diff-b3aa0c81a407f54f636a1cf5a619a4a6) 用来查看本翻译与 AVA 的 master 分支是否有差别（如果你没有看到`code-coverage.md`发生变化，那就意味着这份翻译文档是最新的）。
___

# 代码覆盖率

翻译：[Español](https://github.com/avajs/ava-docs/blob/master/es_ES/docs/recipes/code-coverage.md), [Français](https://github.com/avajs/ava-docs/blob/master/fr_FR/docs/recipes/code-coverage.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/docs/recipes/code-coverage.md), [日本語](https://github.com/avajs/ava-docs/blob/master/ja_JP/docs/recipes/code-coverage.md), [Português](https://github.com/avajs/ava-docs/blob/master/pt_BR/docs/recipes/code-coverage.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/code-coverage.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/code-coverage.md)


因为 AVA[重新处理了测试文件][process-isolation]，所以你不能使用 [`istanbul`] 来做代码覆盖率，但你可以使用 [`nyc`] 来完成，它是支持子进程的 [`istanbul`]。

## 设置

首先安装 NYC：

```
$ npm install nyc --save-dev
```

然后添加`.nyc_output`和`coverage`文件夹到你的`.gitignore`文件。

`.gitignore`:

```
node_modules
coverage
.nyc_output
```

## ES5 覆盖率

使用 NYC 很简单就可以提供使用 ES5 来写的生产代码的覆盖率，只需要在测试脚本前面加上`nyc`：

```json
{
  "scripts": {
    "test": "nyc ava"
  }
}
```

就是这样！

如果你想要创建 HTML 覆盖率报告，或者上传覆盖率数据到 Coveralls，你应该跳过下面的那些章节。

## ES2015 覆盖率

使用 Babel 来转换生产代码有点复杂，这里我们把它分成几个步骤。

### 配置 Babel

首先，我们需要一个 Babel 配置，下面是一个例子，你可以修改它以适应你的需要。

`package.json`:
```json
{
    "babel": {
        "presets": ["es2015"],
        "plugins": ["transform-runtime"],
        "ignore": "test.js",
        "env": {
            "development": {
                "sourceMaps": "inline"
            }
        }
    }
}
```

例子中有 2 点比较重要：

1. 我们忽略测试文件，因为 AVA 已经为你做了转换处理了。

2. 我们为开发环境指定`inline`（内联）原生映射，为了正确的生成覆盖率这个很重要，使用 Babel 配置中的`env`属性可以让我们在生产构建中取消原生映射。


### 创建一个构建脚本

因为你可能不希望在生产代码中`inline`原生映射，你需要在构建脚本中指定一个可代替的环境变量：

`package.json`

```json
{
    "scripts": {
        "build": "BABEL_ENV=production babel --out-dir=dist index.js"
    }
}
```

> 警告：`BABEL_ENV=production`在 Windows 中不可用，你必须使用`set`关键字（`set BABEL_ENV=production`），如果是跨平台构建，请检查 [`cross-env`]。

注意，构建脚本中 AVA 的部分真的很少，它只是一个如何使用 Babel 的`env`配置来操作你的配置以兼容 AVA 的示例。

### 使用 Babel require 钩子

要使用 Babel require 钩子，请在`package.json`中的 AVA 配置里将`require`属性设置为`babel-core/register`。

```json
{
    "ava": {
        "require": ["babel-core/register"]
    }
}
```

*注意*：你也可以在命令行里设置 require 钩子：`ava --require=babel-core/register`。尽管如此，配置在`package.json`里面可以让你不用重复地写标志。

### 把所有东西放在一起

结合上面的步骤，你的`package.json`最后可能是这个样子：

```json
{
    "scripts": {
        "test": "nyc ava",
        "build": "BABEL_ENV=production babel --out-dir=dist index.js"
    },
    "babel": {
        "presets": ["es2015"],
        "plugins": ["transform-runtime"],
        "ignore": "test.js",
        "env": {
            "development": {
                "sourceMaps": "inline"
            }
        }
    },
    "ava": {
        "require": ["babel-core/register"]
    }
}
```


## HTML 报告

NYC 在`.nyc_ouput`文件夹中为每个进程创建一个`json`的覆盖率文件。

把这些文件组合成一个可阅读的 HTML 报告，可以通过下面的方法来做：

```
$ ./node_modules/.bin/nyc report --reporter=html
```

或者，使用 npm 脚本来代替打印命令行：

```json
{
    "scripts": {
        "report": "nyc report --reporter=html"
    }
}
```

这样会在`coverage`文件夹中输出一个 HTML 文件。


## 托管覆盖率报告

### Travis CI & Coveralls

首先，你需要登录 [coveralls.io] 并激活你的项目库。

一旦完成，添加 [`coveralls`] 到开发依赖库：

```
$ npm install coveralls --save-dev
```

然后添加下面的代码到你的`.travis.yml`：

```yaml
after_success:
    - './node_modules/.bin/nyc report --reporter=text-lcov | ./node_modules/.bin/coveralls'
```

你的覆盖率报告将在你的 Travis 完成后很快地出现在 coveralls 上面。

[`babel`]:      https://github.com/babel/babel
[coveralls.io]: https://coveralls.io
[`coveralls`]:  https://github.com/nickmerwin/node-coveralls
[`cross-env`]:  https://github.com/kentcdodds/cross-env
[process-isolation]: https://github.com/avajs/ava-docs/blob/master/zh_CN/readme.md#隔离进程
[`istanbul`]:   https://github.com/gotwarlost/istanbul
[`nyc`]:        https://github.com/bcoe/nyc
