___
**역자주**

이 문서는 [03-assertions](https://github.com/avajs/ava/blob/main/docs/03-assertions.md)의 한국어 번역입니다. [이곳](https://github.com/avajs/ava/compare/71404c23302d825095659c70cb9a1b08251697ad...main#diff-0730bb7c2e8f9ea2438b52e419dd86c9)에서 AVA의 master 브랜치와 이 문서의 차이를 확인할 수 있습니다. (만약 차이가 없다면 문서가 최신 버전임을 의미합니다)
___

# 단언문 (Assertions)

단언문들은 각 테스트 함수 구현에서 [실행 컨텍스트](./02-execution-context.md)를 통해 제공됩니다.

```js
test('unicorns are truthy', t => {
	t.truthy('unicorn'); // 단언문
});
```

단언문들은 그 테스트들에 종속(bounding) 되기 때문에, 실행 컨텍스트에 변수를 할당하거나 넘겨 받을 수 있습니다:

```js
test('unicorns are truthy', t => {
	const truthy = t.truthy;
	truthy('unicorn');
});
```

하나의 테스트 내부에서 여러 단언문들이 실패하게 되면 AVA는 오직 처음 *하나의* 실패만 표시합니다.

단언문은 통과 여부를 나타내는 boolean 값을 리턴합니다. 이 값을 사용해서 테스트를 일찍 종료 할 수 있습니다.

이것이 `throws`, `snapshot`의 경우엔 적용되지 않는다는 점을 유의하세요.

## `plan` 단언문 사용하기

`plan`은 특정 수의 단언문이 실행된 경우에만 테스트를 통과하도록 합니다.

`plan`은 테스트들이 너무 일찍 종료되었을 때 에러를 잡아낼 수 있도록 돕습니다.

`plan`은 또한 너무 많은 단언문이 실행되면 테스트를 실패하게 만들며, 이는 콜백이나 반복문 내부에 단언문이 있는 경우 유용할 수 있습니다.

만약 `plan`을 명시하지 않는다면, 어떤 단언문도 실행되지 않았을 때 여전히 실패할 것입니다.

[`package.json` configuration](./06-configuration.md)의 `failWithoutAssertions` 옵션을 `false`로 만들어 이 behavior를 비활성화 할 수 있습니다.

[`tap`](https://www.npmjs.com/package/tap), [`tape`](https://www.npmjs.com/package/tape)과는 다르게, AVA는 `plan` 단언문에 도달했을 때 자동적으로 테스트를 *종료하지 않습니다.*

다음 예제는 테스트를 통과합니다:

```js
test('resolves with 3', t => {
	t.plan(1);

	return Promise.resolve(3).then(n => {
		t.is(n, 3);
	});
});
```

아래의 경우엔 성공하지 못할 것입니다:

```js
test('loops twice', t => {
	t.plan(2);

	for (let i = 0; i < 3; i++) {
		t.true(i < 3);
	}
}); // 3번의 단언문이 실행되었기 때문에 실패 

test('invokes callback synchronously', t => {
	t.plan(1);

	someAsyncFunction(() => {
		t.pass();
	});
}); // 단언문이 실행되기 전 테스트가 동기적으로 종료되기 때문에 실패.
```

## 단언문 스킵하기

모든 단언문들은 `skip` 한정자를 사용해 스킵될 수 있습니다.

스킵된 단언문들도 여전히 카운트 됩니다, 따라서 plan의 인자를 변경할 필요는 없습니다.

```js
test('skip assertion', t => {
	t.plan(2);
	t.is.skip(foo(), 5); // skip을 추가할 때 plan count를 변경할 필요 없음
	t.is(1, 1);
});
```

## 커스텀 단언문 사용하기

단언문이 실패하고 예외를 throw 하는 경우에는 [built-in 단언문](#built-in-단언문들) 대신 단언문 라이브러리를 사용 할 수 있습니다.

이것은 [built-in 단언문](#built-in-단언문들)을 사용하는 것 만큼 좋은 경험을 주지는 못하겠지만요.

그리고 당신은 [단언문 planning](#plan-단언문-사용하기)을 사용할 수 없게 될 것입니다. (참고: [#25](https://github.com/avajs/ava/issues/25))

AVA는 커스텀 단언문이 성공했는지 알 수 없으므로, 단언문이 실행되지 않은 경우 테스트가 실패하지 않도록 AVA를 설정해야 합니다.

[`package.json` configuration](./06-configuration.md)의 `failWithoutAssertions` 을 false로 설정하세요.

```js
const assert = require('assert');

test('custom assertion', t => {
	assert(true);
});
```

## Built-in 단언문들

### `.pass(message?)`

단언문을 통과시킵니다. 단언문이 성공적으로 패스되었는지 여부를 boolean 값으로 반환합니다.

### `.fail(message?)`

단언문을 실패시킵니다. 단언문이 성공적으로 패스되었는지 여부를 boolean 값으로 반환합니다.

### `.assert(actual, message?)`

`actual`인자가 `truthy`라고 단언합니다. 단언문이 성공적으로 패스되었는지 여부를 boolean 값으로 반환합니다.

### `.truthy(actual, message?)`

`actual`이 `truthy`라고 단언합니다. 단언문이 성공적으로 패스되었는지 여부를 boolean 값으로 반환합니다.

### `.falsy(actual, message?)`

`actual`이 `falsy`라고 단언합니다. 단언문이 성공적으로 패스되었는지 여부를 boolean 값으로 반환합니다.

### `.true(actual, message?)`

`actual`이 `true`라고 단언합니다. 단언문이 성공적으로 패스되었는지 여부를 boolean 값으로 반환합니다.

### `.false(actual, message?)`

`actual`이 `false`라고 단언합니다. 단언문이 성공적으로 패스되었는지 여부를 boolean 값으로 반환합니다.

### `.is(actual, expected, message?)`

`actual`이 `expected`와 같은 결과라고 단언합니다.

이것은 [`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is)에 기반해 판단됩니다.

단언문이 성공적으로 패스되었는지 여부를 boolean 값으로 반환합니다.

### `.not(actual, expected, message?)`

`actual`이 `expected`와 같은 결과가 아니라고 단언합니다.

이것은 [`Object.is()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is)에 기반해 판단됩니다.

단언문이 성공적으로 패스되었는지 여부를 boolean 값으로 반환합니다.

### `.deepEqual(actual, expected, message?)`

`actual`과 `expected`가 깊은 비교를 통해 같은 값(deeply equal) 이라고 단언합니다.

상세 사항은 [Concordance](https://github.com/concordancejs/concordance)을 참조하세요.

### `.notDeepEqual(actual, expected, message?)`

`actual`과 `expected`가 깊은 비교를 통해 다른 값(not deeply equal) 이라고 단언합니다.

`.deepEqual()`의 반대입니다.

단언문이 성공적으로 패스되었는지 여부를 boolean 값으로 반환합니다.

### `.like(actual, selector, message?)`

`actual`이 `selector`와 같다고(like) 단언합니다.

이것은 `.deepEqual()`의 변종으로, `selector`는 `actual`과 동일한 열거형 속성(enumerable properties)을 갖지 않아도 됩니다.

AVA는 `selector`에 깊은 비교를 적용하는 대신, `actual`에서 *comparable* 객체를 이끌어냅니다.

해당 객체를 `selector`와 `.deepEqual()`을 통해 비교합니다.

`selector` 내부의 일반 객체가 아닌 모든 값들은 `actual`의 해당하는 값들과 깊은 비교를 통해 같아야(deeply equal) 합니다.

아래의 예에서, `actual`의 `map` 프로퍼티는 `selector`의 `map` 프로퍼티와 deeply equal 합니다.

그러나, `nested.qux`은 무시됩니다. 해당 값은 `selector`의 열거형 속성에 있지 않기 때문입니다.

```js
t.like({
	map: new Map([['foo', 'bar']]),
	nested: {
		baz: 'thud',
		qux: 'quux'
	}
}, {
	map: new Map([['foo', 'bar']]),
	nested: {
		baz: 'thud',
	}
})
```

최종적으로, 위 테스트는 해당 테스트가 통과 되었음을 나타내는 boolean 값을 리턴하게 됩니다.

### `.throws(fn, expectation?, message?)`

에러가 일어남을 단언합니다.

`fn`은 에러를 던지는 함수여야 합니다.

발생한(thrown) 에러는 반드시 *에러*여야 합니다.

에러가 반환되므로 더 많은 단언문들을 실행할 수 있습니다.

만약 `undefined`가 반환되면 `throws` 단언문은 실패하게 됩니다.

`expectation`은 아래의 프로퍼티들 중 하나 이상의 프로퍼티들을 가질 수 있습니다.

* `instanceOf`: 던져진 에러가 해당 constructor의 인스턴스여야 함을 나타냅니다.
* `is`: 발생한 에러가 `expectation.is`과 엄격하게(strictly) 동일해야 함을 나타냅니다.
* `message`: 문자열인 경우, 발생한 에러의 메세지를 나타내며, 정규표현식인 경우, 발생한 에러의 메시지에 매치 되는지를 나타냅니다.
* `name`: 발생한 에러에 예상되는 `.name` 값 입니다.
* `code`: 발생한 에러에 예상되는 `.code` 값 입니다.

`expectation`은 반드시 명시될 필요는 없습니다.

만약 `message` 이외의 값들을 설정할 필요가 없다면, 다른 값들은 `undefined`로 지정해야 합니다.

예제:

```js
const fn = () => {
	throw new TypeError('🦄');
};

test('throws', t => {
	const error = t.throws(() => {
		fn();
	}, {instanceOf: TypeError});

	t.is(error.message, '🦄');
});
```

### `.throwsAsync(thrower, expectation?, message?)`

에러가 발생함을 단언합니다.

`thrower`는 에러를 던질 async 함수이거나, reject될 프라미스일 수 있습니다. 해당 단언문은 await과 사용되어야 합니다.

발생한(thrown) 값은 *반드시* 에러여야 합니다. 이 함수는 리턴되므로 해당 단언문 외에도 더 많은 단언문을 사용할 수 있습니다.

만약 단언문이 실패한다면, `undefined`가 리턴됩니다.

`expectation`은 아래의 속성들 중 한 개 이상의 속성을 갖는 객체일 수 있습니다:

* `instanceOf`: 발생한 에러 인스턴스의 생성자

* `is`: 발생한 에러는 `expectation.is`와 엄격하게 동일해야(strictly equal) 합니다.

* `message`: 발생한 에러의 문자열이거나, 문자열에 매치되는 정규표현식.

* `name`: 발생한 에러의 예상되는 `.name` 값.

* `code`: 발생한 에러의 예상되는 `.code` 값.

`expectation`은 필수 인자가 아닙니다.

만약 expectation을 원하지 않지만, 단언문 message를 명시하고 싶다면 expectation에 `undefined`를 사용하세요.

예제:

```js
test('throws', async t => {
	await t.throwsAsync(async () => {
		throw new TypeError('🦄');
	}, {instanceOf: TypeError, message: '🦄'});
});
```

```js
const promise = Promise.reject(new TypeError('🦄'));

test('rejects', async t => {
	const error = await t.throwsAsync(promise);
	t.is(error.message, '🦄');
});
```

### `.notThrows(fn, message?)`

fn 내부에서 어떤 에러도 발생하지 않는다고 단언합니다.

`fn`은 에러를 던지지 않는 함수여야 합니다. 해당 함수는 어떤 것도 리턴하지 않습니다.

### `.notThrowsAsync(nonThrower, message?)`

fn 내부에서 어떤 에러도 발생하지 않는다고 단언합니다.

`nonThrower`은 에러를 발생시키지 않는 async 함수이거나, 반드시 resolve 되는 프라미스 일 수 있습니다.

`.throwsAsync()`와 같이, await을 사용해 반드시 해당 단언문이 끝나기를 기다려야 합니다.

```js
test('resolves', async t => {
	await t.notThrowsAsync(promise);
});
```

해당 함수는 어떤 것도 리턴하지 않습니다.

### `.regex(contents, regex, message?)`

`contents`가 정규표현식 `regex`에 매칭 된다고 단언합니다.

해당 단언문이 성공했는지를 나타내는 boolean형 변수를 리턴합니다.

### `.notRegex(contents, regex, message?)`

`contents`가 정규표현식 `regex`에 매칭 되지 않는다고 단언합니다.

해당 단언문이 성공했는지를 나타내는 boolean형 변수를 리턴합니다.

### `.snapshot(expected, message?)`

`expected`값과 이전에 기록된 스냅샷을 비교합니다.

스냅샷은 매 테스트 마다 저장됩니다, 따라서 당신의 테스트들에 고유한 title 값을 명시하세요.

### `.try(title?, implementation | macro, ...args?)`

`.try()`는 테스트를 실패시키지 않으면서 단언문을 *시도* 해 볼 수 있도록 합니다.

함수 구현은 다른 테스트 함수들과 동일한 방식으로 작동합니다.

매크로도 사용할 수 있습니다.

첫 번째 title 인자는 항상 선택적(optional) 입니다.

추가 인자들은 macro 함수나 구현 함수에 넘겨 집니다.

`.try()`는 async 함수로, `await`과 함께 사용되어야 합니다.

리턴된 객체는 `commit()`, `discard()` 메서드를 갖습니다.

검사 결과를 커밋할 지(commit), 버릴 지(discard) 결정해야 합니다.

실패한 결과를 커밋한다면, 테스트는 실패하게 됩니다.

`passed` 프로퍼티를 통해 해당 시도가 패스 되었는지 여부를 체크할 수 있습니다.

단언문들의 모든 에러는 `errors` 프로퍼티를 통해 사용할 수 있습니다.

해당 시도의 title 값은 `title` 프로퍼티를 통해 사용할 수 있습니다.

`t.log()`를 통한 로그 값들은 `logs` 프로퍼티를 통해 사용 가능합니다.

이런 로그 값들을 테스트의 일부로 유지할지 `{retainLogs: true}`를 `commit()`, `discard()` 메서드들에 넘겨줌으로써 선택할 수 있습니다.

함수 구현은, 테스트 함수처럼 그 함수 자체의 [실행 컨텍스트](./02-execution-context.md)를 받습니다.

시도의 실행 컨텍스트를 사용해 단언문만 수행하도록 주의해야 합니다.

테스트가 통과되려면, 시도들에서 적어도 하나의 단언문은 반드시 패스되어야 합니다.

하나의 테스트 내부에서 동시에(concurrently) 여러 시도들을 실행할 수 있습니다.

(그러나, 이렇게 할 땐 snapshot을 사용할 수 없습니다.)

예제:

```js
const twoRandomIntegers = () => {
	const rnd = Math.round(Math.random() * 100);
	const x = rnd % 10;
	const y = Math.floor(rnd / 10);
	return [x, y];
};

test('flaky macro', async t => {
	const firstTry = await t.try((tt, a, b) => {
		tt.is(a, b);
	}, ...randomIntegers());

	if (firstTry.passed) {
		firstTry.commit();
		return;
	}

	firstTry.discard();
	t.log(firstTry.errors);

	const secondTry = await t.try((tt, a, b) => {
		tt.is(a, b);
	}, ...randomIntegers());
	secondTry.commit();
});
```

해당 단언문이 성공했는지를 나타내는 boolean형 변수를 리턴합니다.
