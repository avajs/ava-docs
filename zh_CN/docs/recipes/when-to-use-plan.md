___
**备注**

这是 [when-to-use-plan.md](https://github.com/sindresorhus/ava/blob/master/docs/recipes/when-to-use-plan.md) 的简体中文翻译。这个[链接](https://github.com/sindresorhus/ava/compare/89767ec3b6174e59d37faaadb50cfa3c0d58bda6...master#diff-0c25d982e94d600cb6b8e438a0e67169) 用来查看本翻译与 AVA 的 master 分支是否有差别（如果你没有看到`when-to-use-plan.md`发生变化，那就意味着这份翻译文档是最新的）。
___

# 什么时候使用`t.plan()`

翻译：[Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/docs/recipes/when-to-use-plan.md), [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/docs/recipes/when-to-use-plan.md), [Italiano](https://github.com/sindresorhus/ava-docs/blob/master/it_IT/docs/recipes/when-to-use-plan.md), [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/docs/recipes/when-to-use-plan.md),  [Português](https://github.com/sindresorhus/ava-docs/blob/master/pt_BR/docs/recipes/when-to-use-plan.md), [Русский](https://github.com/sindresorhus/ava-docs/blob/master/ru_RU/docs/recipes/when-to-use-plan.md), [简体中文](https://github.com/sindresorhus/ava-docs/blob/master/zh_CN/docs/recipes/when-to-use-plan.md)

AVA 和 [`tap`](https://github.com/tapjs/node-tap)/[`tape`](https://github.com/substack/tape) 的一个主要不同是`t.plan()`的行为。在 AVA 中`t.plan()`只是用来断言期望的断言数是否正确，并不会自动结束测试。

## `t.plan()`不好的用法

很多从`tap/tape`过渡过来的用户习惯在每个测试里大量使用`t.plan()`，尽管如此，在 AVA 里我们不认为这是一个“最佳实践”，我们反而认为`t.plan()`只有在少数情况才能提供其价值。

### 没有分支的同步测试

在大部分同步测试中`t.plan()`是没有必要的。

```js
test(t => {
    // 不好：这里没有分支，t.plan 是无意义的
    t.plan(2);

    t.is(1 + 1, 2);
    t.is(2 + 2, 4);
});
```

`t.plan()`在这里并没有提供任何价值，并且如果你决定添加或删除断言时会带来额外的工作。

### Promises 期望得到 resolve

```js
test(t => {
    t.plan(1);

    return somePromise().then(result => {
        t.is(result, 'foo');
    });
});
```

简单看一下，这个测试充分利用了`t.plan()`的优势，因为这里面涉及到了异步 promise 处理。尽管如此在这个测试里还有一些问题：

1. `t.plan()`用在这里大概是为了防止`somePromise()`可能被 reject 的情况，但返回一个 reject 的 promise 测试始终会失败。

2. 使用`async`/`await`可能会更好。

```js
test(async t => {
    t.is(await somePromise(), 'foo');
});
```

## `t.plan()`好的用法

`t.plan()`有很多可接受的用法。

### Promise 带上一个`.catch()`块

```js
test(t => {
    t.plan(2);

    return shouldRejectWithFoo().catch(reason => {
        t.is(reason.message, 'Hello') // 如果你关心的是 message 的话使用 t.throws() 更好
        t.is(reason.foo, 'bar');
    });
});
```

在这里，`t.plan()`用来确保`catch`块中的代码被执行，在大部分情况下，你更应该使用`t.throws()`断言，但这是一个可以接受的用法，因为`t.throws()`只允许你断言错误的`message`属性。

### 确保一个 catch 语句发生

```js
test(t => {
    t.plan(2);

    try {
        shouldThrow();
    } catch (err) {
        t.is(err.message, 'Hello') // 如果你关心的是 message 的话使用 t.throws() 更好
        t.is(err.foo, 'bar');
    }
});
```

像上面这种`try`/`catch`的情况，使用`t.throws()`一般是个更好的选择，但它只允许你断言错误的`message`属性。

### 确保多个 callback 被真正调用

```js
test.cb(t => {
    t.plan(2);

    const callbackA = () => {
        t.pass();
        t.end();
    };

    const callbackB = () => t.pass();

    bThenA(callbackA, callbackB);
});
```

上面确保`callbackB`先被调用（且只调用一次），紧接着调用`callbackA`，其他的组合不会满足计划。

### 测试带有分支语句

在大部分情况下，在测试中使用复杂的分支是一个坏主意，一个明显的例外是用来测试自动生成的东西（可能来自一个 JSON 文档）。下面的`t.plan()`用来确保 JSON 输入的正确性：

```js
const testData = require('./fixtures/test-definitions.json');

testData.forEach(testDefinition => {
    test(t => {
        const result = functionUnderTest(testDefinition.input);

        // testDefinition 应该有一个`foo`或`bar`的预期而不是同时满足它们
        t.plan(1);

        if (testDefinition.foo) {
            t.is(result.foo, testDefinition.foo);
        }

        if (testDefinition.bar) {
            t.is(result.bar, testDefinition.foo);
        }
    });
});
```

## 总结

`t.plan()`有很多有效的使用方法，但不应该被盲目使用。一个好的经验法则是你的*测试*没有简单的，容易推断的，代码流的话你可以使用它。测试在 callback 里带有断言，`if`/`then`语句，`for`/`while`循环，并且（在一些情况下）`try`/`catch`块，都可以考虑使用`t.plan()`。
