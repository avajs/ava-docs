___
**역자주**

이 문서는 [when-to-use-plan.md](https://github.com/avajs/ava/blob/main/docs/recipes/when-to-use-plan.md)의 한국어 번역입니다. [이곳](https://github.com/avajs/ava/compare/71404c23302d825095659c70cb9a1b08251697ad...main#diff-0730bb7c2e8f9ea2438b52e419dd86c9)에서 AVA의 master 브랜치와 이 문서의 차이를 확인할 수 있습니다. (만약 차이가 없다면 문서가 최신 버전임을 의미합니다)
___

# `t.plan()`을 사용해야 할 때

AVA 와 [`tap`](https://github.com/tapjs/node-tap)/[`tape`](https://github.com/substack/tape)의 가장 주요한 차이점들 중 하나는 `t.plan()`의 행동(behavior) 입니다.

AVA에선, `t.plan()`는 특정 단언문(assertion)이 몇 번이나 호출되었는지를 세기 위해서만 사용됩니다. `t.plan`은 테스트를 자동으로 종료시키지 않습니다.

## `t.plan()`의 오용

`tap`/`tape`를 사용했었던 많은 유저들은 `t.plan()`을 모든 테스트에 사용하는 것에 익숙합니다. 그러나 AVA에서는 이를 "모범 사례"로 간주하지 않습니다.

우리는 대신 `t.plan()`을 오직 `t.plan()`이 특정 값을 제공하는 경우에만 사용해야 한다고 믿습니다.

### 분기문 없는 sync 테스트의 경우

`t.plan()` 은 거의 모든 sync 테스트들에서 불필요합니다.

```js
test('simple sums', t => {
	// BAD: 여기에 분기는 없음 - t.plan()을 사용하는 게 의미가 없음.
	t.plan(2);

	t.is(1 + 1, 2);
	t.is(2 + 2, 4);
});
```

`t.plan()`은 여기서 어떤 값도 제공하지 않으며, 단언문을 추가하거나 제거할 때만 추가 작업을 생성합니다.

### resolve 될 것으로 예상되는 프라미스의 경우

```js
test('gives foo', t => {
	t.plan(1);

	return somePromise().then(result => {
		t.is(result, 'foo');
	});
});
```

딱 보기에 비동기 프라미스 핸들러가 포함되어 있으므로 이 테스트는 `t.plan()`을 잘 활용하는 것으로 보입니다.

그러나 해당 테스트는 아래와 같은 문제점들을 갖고 있습니다.

1. 여기서 `t.plan()`은 `somePromise()`가 reject될 가능성을 방지하기 위해 사용되는 것으로 추정되지만, reject 된 프라미스를 반환하는 것은 어쨌든 테스트를 실패시킵니다.

2. `async`/`await`의 이점을 이용하는 편이 낫습니다:

```js
test('gives foo', async t => {
	t.is(await somePromise(), 'foo');
});
```

### `.catch()` 블록을 사용하는 프라미스의 경우

```js
test('rejects with foo', t => {
	t.plan(2);

	return shouldRejectWithFoo().catch(reason => {
		t.is(reason.message, 'Hello');
		t.is(reason.foo, 'bar');
	});
});
```

위 코드에서, `t.plan()`는 `catch` 블록 내부의 코드가 실행되는 것을 확실히 실행되도록 보장하는 것으로 보입니다.

그 대신, `t.throwsAsync`와 `async`/`await`의 이점을 이용해, 아래와 같은 더 나은 코드를 작성해야 합니다.

```js
test('rejects with foo', async t => {
	const reason = await t.throwsAsync(shouldRejectWithFoo());
	t.is(reason.message, 'Hello');
	t.is(reason.foo, 'bar');
});
```

### catch 구문이 실행되도록 보장하기

```js
test('throws', t => {
	t.plan(2);

	try {
		shouldThrow();
	} catch (err) {
		t.is(err.message, 'Hello');
		t.is(err.foo, 'bar');
	}
});
```

위 예에서 설명한 것처럼, `t.throws()`와 `async`/`await`을 사용하는 것이 더 나은 선택입니다.

## `t.plan()`의 올바른 사용 예제

`t.plan()` 은 아래와 같은 값을 제공합니다.

### 분기문 테스트

대부분의 경우 테스트에 복잡한 분기문을 사용하는 것은 좋지 않습니다.

언급할만한 예외 사항은 자동으로 생성되는 테스트에 대한 것 (아마도 JSON 문서에서) 입니다. 아래 `t.plan()`은 JSON 파일의 정확성을 보장하는 데 사용됩니다.

예제:

```js
import fs from 'fs';
import path from 'path';

const testData = JSON.parse(fs.readFileSync(new URL('./fixtures/test-definitions.json', import.meta.url)));

for (const testDefinition of testData) {
	test('foo or bar', t => {
		const result = functionUnderTest(testDefinition.input);

		// testDefinition은 `foo`나 `bar` 단언문이 실행될 것이라고 기대하지만, 둘 다 실행되기를 기대하지 않음.
		t.plan(1);

		if (testDefinition.foo) {
			t.is(result.foo, testDefinition.foo);
		}

		if (testDefinition.bar) {
			t.is(result.bar, testDefinition.foo);
		}
	});
}
```

## 결론

`t.plan()`은 여러 가지 유효한 용도가 있지만 무분별하게 사용해서는 안 됩니다.

경험적으로 볼 때 좋은 원칙 중 하나는 *테스트*가 쉽게 추론 가능한 분기문 / 제어문 / (경우에 따라) `try`/`catch`문을 갖는 경우 사용하는 것 입니다.

콜백 내부에서 단언문을 사용하는 테스트, `if`/`then` 문, `for`/`while` 루프 및 (경우에 따라) `try`/`catch` 블록은 모두 `t.plan()`에 적합한 후보입니다.
