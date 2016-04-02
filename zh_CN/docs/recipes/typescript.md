___
**备注**

这是 [typescript.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/typescript.md) 的简体中文翻译。这个[链接](https://github.com/sindresorhus/ava/compare/8e2f3dca177a4283ad882596d3c1425cabb998ef...master#diff-60cce07a584082115d230f2e3d571ad6) 用来查看本翻译与 AVA 的 master 分支是否有差别（如果你没有看到`typescript.md`发生变化，那就意味着这份翻译文档是最新的）。
___

# TypeScript

翻译：[Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/typescript.md), [Italiano](https://github.com/sindresorhus/ava-docs/blob/master/it_IT/docs/recipes/typescript.md), [Русский](https://github.com/sindresorhus/ava-docs/blob/master/ru_RU/docs/recipes/typescript.md), [简体中文](https://github.com/sindresorhus/ava-docs/blob/master/zh_CN/docs/recipes/typescript.md)

AVA 捆绑了一个 TypeScript 定义文件，让开发人员可以了解如何用 TypeScript 写测试。

## 设置

首先安装 TypeScript 编译器 [tsc](https://github.com/Microsoft/TypeScript)。

```
$ npm install --save-dev tsc
```

创建一个 [`tsconfig.json`](https://github.com/Microsoft/TypeScript/wiki/tsconfig.json) 文件，文件指定编译器是用来编译工程或者测试文件。

```json
{
    "compilerOptions": {
        "module": "commonjs",
        "target": "es2015"
    }
}
```

在`package.json`文件里添加一个`test`脚本，在运行 AVA 前先编译工程。

```json
{
  "scripts": {
    "test": "tsc && ava"
  }
}
```


## 添加测试

创建一个`test.ts`文件。

```ts
import test from 'ava';

async function fn() {
    return Promise.resolve('foo');
}

test(async (t) => {
    t.is(await fn(), 'foo');
});
```


## 执行测试

```
$ npm test
```
