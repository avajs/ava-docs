___
**역자주**

이 문서는 [01-writing-tests.md](https://github.com/avajs/ava/blob/main/docs/01-writing-tests.md)의 한국어 번역입니다. [이곳](https://github.com/avajs/ava/compare/71404c23302d825095659c70cb9a1b08251697ad...main#diff-0730bb7c2e8f9ea2438b52e419dd86c9)에서 AVA의 master 브랜치와 이 문서의 차이를 확인할 수 있습니다. (만약 차이가 없다면 문서가 최신 버전임을 의미합니다)
___

# 테스트 작성하기

테스트들은 동시에(concurrently) 실행됩니다.

동기적으로(synchronously) 실행될 테스트와 비동기적으로(asynchronously) 실행될 테스트들을 명시할 수 있습니다.

테스트들은 프라미스(promise)나 [observable](https://github.com/zenparsing/zen-observable)를 반환하거나, 콜백 테스트로 선언하지 않는 한 동기적으로 취급됩니다.

모든 테스트들을 동기적으로(synchronously) 정의해야 합니다.

해당 테스트들은 `setTimeout`, `setImmediate` 등의 콜백 함수 내부에 정의될 수 없습니다.

AVA는 테스트 파일들을 `package.json`을 포함하는 현재 디렉토리(cwd)에서 실행하려고 시도합니다.

## 테스트 고립 (isolation)

각각의 파일들은 새 워커 스레드(worker thread)에서 실행됩니다.

이것은 AVA 4의 새 기능으로, AVA 3에선 폴백(fallback) 으로 각각의 테스트들이 별개의 프로세스에서 실행됩니다.

AVA는 `NODE_ENV` 환경변수가 따로 지정되지 않았다면, `process.env.NODE_ENV`를 `test`로 지정합니다.

이것은 테스트 하려는 코드가 디폴트 값들을 갖고 있는 경우 유용합니다. (예를 들어, 어떤 데이터베이스에 연결할 지 선택할 때)

하지만 이것은 코드나 코드의 디펜던시가 다른 방식으로 동작하게 만드는 원인이 될 수도 있습니다.

`process.env`의 `NODE_ENV`는 항상 `true`가 될 것임에 유의하세요.

## 테스트 선언하기

테스트를 선언하기 위해, AVA로 부터 `test` 함수를 import 해 호출하세요.

필요한 title 및 함수 구현을 제공하세요.

title은 각각의 테스트 파일 내부에서 고유한 값이어야 합니다.

테스트 함수는 테스트 파일이 실행될 때 실행되며, [execution object](./02-execution-context.md)를 첫 번째 인자로 넘겨 받습니다.

```js
import test from 'ava';

test('my passing test', t => {
	t.pass();
});
```

## 순차적으로 테스트 실행하기

테스트들은 기본적으로, 병렬적으로 실행됩니다.

그러나 가끔씩 테스트들을 병렬적으로 실행해선 안 되는 경우가 있습니다.

이런 드문 경우에, `.serial` 제어자를 사용하세요.

이 제어자는 해당 테스트들이 *동시적인 테스트들이 실행되기 전*에, 순차적으로 실행되게 만듭니다.

```js
test.serial('passes serially', t => {
	t.pass();
});
```

이 한정자는 특정 테스트 파일 내부의 테스트들에만 적용됨에 유의하세요.

AVA는 여전히 [`--serial` CLI flag](./05-command-line.md)를 커맨드 라인 인자로 넘기지 않는 한, 동시에 여러 테스트 파일들을 실행할 것입니다.

`.serial`은 모든 테스트, hook과 심지어, `.todo()` 에서 사용 가능하지만, `test` 함수에만 사용 가능합니다.

## 프라미스 (Promise) 지원

테스트들은 프라미스를 반환할 수 있습니다.

AVA는 test가 끝나기 전, 프라미스가 resolve 될 때 까지 기다립니다.

만약 프라미스가 reject 되면, 테스트는 실패할 것입니다.

```js
test('resolves with unicorn', t => {
	return somePromise().then(result => {
		t.is(result, 'unicorn');
	});
});
```

## Async 함수 지원

AVA는 [Async 함수](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)을 지원합니다.

```js
test(async function (t) {
	const value = await promiseFn();
	t.true(value);
});

// 비동기 arrow function
test('promises the truth', async t => {
	const value = await promiseFn();
	t.true(value);
});
```

## Observable 지원

AVA는 [observables](https://github.com/zenparsing/es-observable)를 지원합니다.

만약, 테스트에서 observable를 반환한다면, AVA는 테스트가 끝나기 전, 자동적으로 해당 observable를 마지막까지 소비(consume) 합니다.

*"callback mode"를 사용하거나 `t.end()`를 호출할 필요가 없습니다.*

```js
test('handles observables', t => {
	t.plan(3);
	return Observable.of(1, 2, 3, 4, 5, 6)
		.filter(n => {
			// 짝수만 리턴
			return n % 2 === 0;
		})
		.map(() => t.pass());
});

```
## 특정 테스트들 실행하기

개발 도중, 특정 테스트만 실행시키는 것은 유용할 수 있습니다.

이것은 `.only` 한정자를 사용해 이뤄질 수 있습니다.

```js
test('will not be run', t => {
	t.fail();
});

test.only('will be run', t => {
	t.pass();
});
```

모든 테스트들에 `.only` 한정자를 사용할 수 있습니다.

hooks나, `.todo()`에는 사용할 수 없습니다.

*주의:* `.only` 한정자는 해당 파일 내부에서만 적용됩니다, 즉 만약 여러 테스트 파일들을 실행시킨다면, 다른 테스트 파일에 속한 테스트들은 여전히 모두 실행될 것입니다. 만약, `test.only` 테스트만 실행시키고 싶다면, 그냥 해당 테스트 파일을 AVA에 넘기세요.

## 테스트 스킵하기

가끔 실패하는 테스트들을 고치기 어려운 경우가 있습니다.

`.skip` 한정자를 사용해 임시적으로 해당 테스트들을 스킵할 수 있습니다.

여전히 출력에는 나타나지만, 이 테스트들은 절대 실행되지 않습니다.

```js
test.skip('will not be run', t => {
	t.fail();
});
```

함수의 구현을 명시해야 합니다.

`.skip` 한정자는 모든 테스트들과 hooks에 사용 가능하지만, `.todo()`엔 사용할 수 없습니다.

`.skip`에 다른 한정자를 적용할 수도 없습니다.

만약 테스트가 한 동안 계속 실패할 가능성이 높다면, 대신 `.failing()`을 사용하세요.

## 테스트 플레이스홀더 ("todo")

테스트를 작성할 계획일 때, `.todo` 한정자를 사용할 수 있습니다.

스킵된 테스트들과 같이, 이 플레이스홀더들은 출력에 나타납니다.

해당 테스트들은 title만을 필요로 합니다. 즉, 함수 구현 없이 해당 테스트 플레이스홀더들을 명시할 수 있습니다.

```js
test.todo('will think about writing this later');
```

만약 순차적인 (serial) 테스트를 작성해야 한다면, 아래처럼 작성할 수 있습니다.

```js
test.serial.todo('will think about writing this later');
```

## 실패 중인 (Failing) 테스트들

`.failing` 한정자를 사용해, 고쳐져야 하는 이슈들을 코드와 함께 문서화 할 수 있습니다.

해당 테스트들은 일반적인 테스트들과 동일하게 실행되지만, 실패할 것으로 예상됩니다.

그리고 이 테스트들의 실패는 빌드를 방해하지 않을 것입니다.

만약 failing으로 표시된 테스트가 성공적으로 실행되면, 빌드는 실패하게 되며, `.failing` 한정자를 제거하라는 유용한 지시 메시지와 함께 에러로 보고됩니다.

이것은 해당 이슈가 고쳐지기 전, CI failing 없이 `.failing` 테스트들을 병합할 수 있게 해 줍니다.

이것은 버그 리포터가 해당 이슈를 해결할 수 없는 경우에도, 버그 보고를 PR로 만들 수 있는 훌룡한 방법입니다.

```js
// github.com/user/repo/issues/1234 이슈를 참고하세요.
test.failing('demonstrate some bug', t => {
	t.fail(); // 테스트는 성공한 것으로 카운트 될 것 입니다.
});
```

## Before & after hooks

AVA는 테스트들이 실행되기 전, 후에 실행될 hook들을 등록할 수 있도록 만들어줍니다.

이것은 사전 작업(setup), 테스트 후 처리(teardown) 코드를 작성할 수 있도록 해 줍니다.

`test.before()` 는 테스트 파일에서 첫 번째 테스트 전 실행될 hook을 등록합니다.

비슷하게, `test.after()`는 마지막 테스트 후 실행될 hook을 등록합니다.

각 테스트, 다른 hook들이 종료될 때 마다 **항상** 한 번씩 실행될 hook을 `test.after.always()`로 등록하세요.

`.always()` hook은 이전에 일부 테스트가 실패했었더라도 실행되므로, 정리(cleanup) 작업에 이상적입니다.

그러나 uncaught exceptions, unhandled rejections이나 timeouts이 크래시를 내는 경우, `.always()` hook은 실행되지 않을 수 있습니다.

`test.beforeEach()` 는 테스트 파일에서 각 테스트들 이전에 실행될 hook을 등록합니다.

비슷하게 `test.afterEach()`는 각 테스트들 이후에 실행될 hook을 등록합니다.

`test.afterEach.always()`로 다른 테스트, hook이 실패하더라도 실행될 hook을 등록할 수 있습니다.

만약 테스트가 `.skip` 한정자와 함께 스킵된다면, 스킵된 테스트에 해당하는 `.beforeEach()`, `.afterEach()`, `.afterEach.always()` 각각은 실행되지 않습니다.

비슷하게, 테스트 파일 내 모든 테스트가 스킵되는 경우, `.before()`, `.after()` and `.after.always()`는 실행되지 않습니다.

*당신은 clean up 처리를 위해 `.afterEach.always()`를 사용할 필요가 없을 수도 있습니다.* [`t.teardown()`](./02-execution-context.md#tteardownfn)를 사용해 특정 테스트 *내부*의 사이드 이펙트들을 지울 수 있습니다.

`test()`처럼, 이 메서드들은 선택적인(optional) title 인자, 구현 함수를 인자로 넘겨 받습니다.

hook이 실행에 실패하게 되면 title이 표시됩니다.

구현 함수는 [execution object](./02-execution-context.md)와 함께 실행됩니다.

단언문(assertions)을 hook 내부에 사용할 수 있습니다.

또한 [macro function](#매크로-함수를-통해-재사용-가능한-테스트-로직-만들기) 및 추가적인 인자들을 넘겨 줄 수 있습니다.

`.before()` hook들은 `.beforeEach()` hook 보다 먼저 실행됩니다.

`.afterEach()` hook들은 `.after()` 이후에 실행됩니다.

그들의 카테고리 내부에서, hook 들은 그들이 정의된 순서대로 실행됩니다.

기본적으로 hook들은 동시적으로(concurrently) 실행되지만, `test.serial`을 사용해 하나씩 실행되도록 보장할 수 있습니다.

테스트들과 달리, 연속적인 hook들(serial hook)은 다른 concurrent한 hook들 이전에 *실행되지 않습니다*.

```js
test.before(t => {
	// 이 hook은 모든 테스트들 전에 실행됩니다.
});

test.before(t => {
	// 이 hook은 위 hook과 동시에 (concurrently) 실행됩니다.
});

test.serial.before(t => {
	// 이 hook은 위 hook이 끝난 후 실행됩니다.
});

test.serial.before(t => {
	// 이 hook 또한 위 hook이 끝난 후, 그리고 테스트들이 실행되기 전 실행됩니다.
});

test.after('cleanup', t => {
	// 이 hook은 모든 테스트들 후에 실행됩니다.
});

test.after.always('guaranteed cleanup', t => {
	// 이 hook은 이전 실패에 관계 없이 항상 실행됩니다.
});

test.beforeEach(t => {
	// 이 hook은 각 테스트들의 실행 전 실행됩니다. 
});

test.afterEach(t => {
	// 이 hook은 각 테스트들의 실행 후에 실행됩니다.
});

test.afterEach.always(t => {
	// 이 hook은 각 테스트들, 다른 테스트 hook들의 실행 이후에 실행되며, 심지어 그것들이 fail 되었더라도 실행됩니다. 
});

test('title', t => {
	// 테스트
});
```

hook들은 테스트처럼 동기적(synchronous) 일 수도, 비동기적(asynchronous) 일 수도 있습니다.

hook을 비동기로 만들기 위해, 프라미스나 observable을 리턴하거나, async 함수를 사용하세요.

```js
test.before(async t => {
	await promiseFn();
});

test.after(t => {
	return new Promise(/* ... */);
});
```

`.beforeEach()`, `.afterEach()` hook들은 각 테스트가 실행되기 바로 전, 후에 실행되며, 기본적으로 테스트들은 병렬적으로 실행된다는 것을 명심하세요.

이것은 여러 `.beforeEach()` hook들이 병렬적으로 실행됨을 의미합니다.

`test.serial.beforeEach()`를 사용해도, 이것은 변하지 않습니다.

만약 이 예제처럼 ([`console.log` 추적하기](https://github.com/avajs/ava/issues/560)) 전역 상태를 설정(set up)할 필요가 있다면, 해당 테스트들이 그 자체로 [연속적으로 실행되도록](#순차적으로-테스트-실행하기) 만들어야 합니다.

AVA는 각 테스트 파일들을 해당 프로세스 내부에서 실행한다는 것을 기억하세요.

`.after()` hook은 프로세스 종료 바로 이전에만 호출되기 때문에, 해당 hook에서 전역 상태를 정리할 필요가 없을 수 있습니다.

## 테스트 컨텍스트 (context)

hook들은 해당 테스트 내부에서 컨텍스트를 공유합니다.

```js
test.beforeEach(t => {
	t.context.data = generateUniqueData();
});

test('context data is foo', t => {
	t.is(t.context.data + 'bar', 'foobar');
});
```

`.before` hook이 `t.context`를 object로 취급한다면, `.beforeEach()` hook 및 테스트들이 호출 될 때 얕은 복사 (shallow copy)가 일어납니다.

다른 값 타입들은 그대로(copy by value) 전달됩니다.

`.after()`와 `.after.always()` hook들은 context 원본 값을 전달 받습니다.

`.beforeEach()`, `.afterEach()`, `.afterEach.always()` hook 들의 경우, context는 서로 다른 테스트들 간에 *공유되지 않습니다.*

이것은 당신이 다른 테스트들에 leak을 일으키지 않도록 데이터를 셋업할 수 있게 해 줍니다.

기본적으로 `t.context`는 object 이지만, 재할당할 수 있습니다:

```js
test.before(t => {
	t.context = 'unicorn';
});

test('context is unicorn', t => {
	t.is(t.context, 'unicorn');
});
```

## 테스트 메타 데이터

현재 로딩 되어 있는 테스트 파일에 대한 정보에 `test.meta`를 통해 접근 가능합니다.

사용 가능한 속성들:

* `file`: 테스트 파일로의 경로 (URL 문자열)
* `snapshotDirectory`: 스냅샷 파일들이 저장되는 디렉토리 (URL 문자열)

```js
import test from 'ava';

console.log('동시에 실행되는 테스트 파일 (Test file currently being run):', test.meta.file);
```

## 매크로 함수를 통해 재사용 가능한 테스트 로직 만들기

[![StackBlitz 예제 코드 보기](https://developer.stackblitz.com/img/open_in_stackblitz.svg)](https://stackblitz.com/github/avajs/ava/tree/main/examples/macros?file=test.js&terminal=test&view=editor)

테스트 선언 (declaration)에 전달되는 추가 매개변수들은 테스트 함수로 전달됩니다.

이것은 재사용 가능한 테스트 매크로를 만드는 것에 유용합니다.

간단한 함수(plain functions)를 사용해 아래처럼 _사용할 수 있습니다_:

```js
function macro(t, input, expected) {
	t.is(eval(input), expected);
}

test('2 + 2 = 4', macro, '2 + 2', 4);
test('2 * 3 = 6', macro, '2 * 3', 6);
```

그러나 `test.macro()`를 사용해 아래처럼 사용하는 것이 선호됩니다:

```js
import test from 'ava';

const macro = test.macro((t, input, expected) => {
	t.is(eval(input), expected);
});

test('title', macro, '3 * 3', 9);
```

아니면 title 함수와 함께 아래처럼 사용할 수 있습니다:

```js
import test from 'ava';

const macro = test.macro({
	exec(t, input, expected) {
		t.is(eval(input), expected);
	},
	title(providedTitle = '', input, expected) {
		return `${providedTitle} ${input} = ${expected}`.trim();
	},
});

test(macro, '2 + 2', 4);
test(macro, '2 * 3', 6);
test('providedTitle', macro, '3 * 3', 9);
```

`providedTitle` 인자는 기본적으로 사용자가 값을 제공해주지 않는 경우, `undefined`가 됩니다.

이것은 디폴트 매개 변수(default function parameter) 할당이 가능하다는 것을 의미합니다.

위 예에선 빈 문자열을 디폴트 값으로 사용합니다.
