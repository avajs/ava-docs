___
**역자주**

이 문서는 [08-common-pitfalls.md](https://github.com/avajs/ava/blob/main/docs/08-common-pitfalls.md)의 한국어 번역입니다. [이곳](https://github.com/avajs/ava/compare/71404c23302d825095659c70cb9a1b08251697ad...main#diff-0730bb7c2e8f9ea2438b52e419dd86c9)에서 AVA의 master 브랜치와 이 문서의 차이를 확인할 수 있습니다. (만약 차이가 없다면 문서가 최신 버전임을 의미합니다)
___

# 빠지기 쉬운 함정

만약 [ESLint](https://eslint.org)를 사용 중이라면, [eslint-plugin-ava](https://github.com/avajs/eslint-plugin-ava) 설치를 고려할 수 있습니다. 이 플러그인은 AVA를 올바르게 사용하고 몇 가지 일반적인 함정들을 피할 수 있도록 도와줄 수 있습니다.

## Docker에서의 AVA

AVA를 Docker 내부에서 CI의 일부로 사용하는 경우, 환경 변수들을 적절하게 수정해주어야 합니다.

구체적으로, `docker exec` 명령에 `-e CI=true`룰 추가하세요. (참고: [#751](https://github.com/avajs/ava/issues/751))

AVA는 [is-ci](https://github.com/watson/is-ci)를 사용해 AVA가 CI 환경에 있는지, 혹은 [이러한 변수들](https://github.com/watson/ci-info/blob/master/index.js)을 사용하지 않는지 검사합니다.

## AVA와 연결된 클라이언트 수 제한

당신은 제한된 수의 동시 연결만 허용하는 서비스를 사용하고 있을 수 있습니다.

예를 들어, 많은 데이터베이스 서비스(database-as-a-service)들은 동시에 사용할 수 있는 클라이언트 수에 제한이 있는 무료 요금제를 제공합니다.

AVA는 다수의 프로세스를 실행하기 때문에 이러한 한계에 도달할 수 있습니다, 잘 작성된 서비스는 그러한 경우 에러나 스로틀(throttle)을 발생시킬 것입니다.

사용 중인 제품이 그렇지 않은 경우, 테스트가 중단될 것입니다.

기본적으로, AVA는 기계의 [논리 코어 갯수](https://superuser.com/questions/1105654/logical-vs-physical-cpu-performance) 만큼의 프로세스를 사용합니다.

이 값은 CI 환경에선, 2로 제한됩니다.

실행될 프로세스 수를 제한을 변경하려면, `concurrency` 플래그를 사용하세요.

예를 들어, 당신의 서비스 요금제에 5개의 클라이언트가 허용되는 경우 `concurrency=5`, 또는 그 이하로 AVA를 실행해야 합니다.

## 비동기 작업

테스트 내에서 비동기 작업을 실행하고 있는데 왜 완료되지 않는지 궁금할 수 있습니다.

비동기 작업에서 프라미스를 사용하는 경우 해당 프라미스를 반환해야 합니다.

```js
test('fetches foo', t => {
	return fetch().then(data => {
		t.is(data, 'foo');
	});
});
```

더 좋은 방법은 `async` / `await`을 사용하는 것입니다.

```js
test('fetches foo', async t => {
	const data = await fetch();
	t.is(data, 'foo');
});
```

만약 콜백 함수를 사용하고 있다면, [`util.promisify()`](https://nodejs.org/dist/latest/docs/api/util.html#util_util_promisify_original) 같은 유틸리티 함수를 사용해 콜백 함수를 프라미스화(promisify) 하세요.

```js
const {promisify} = require('util');

test('fetches foo', async t => {
	const data = await promisify(fetch)();
	t.is(data, 'foo');
});
```

## 잡히지 않는 예외 처리하기

AVA는 [uncaught exceptions을 추적할 수 없습니다](https://github.com/avajs/ava/issues/214).

콜백 함수(callback)를 인자로 받는 함수들은 디버깅하기 힘든, 잡히지 않는 예외를 던지는 결과를 낳을 수 있습니다.

위 예제처럼 해당 함수를 프라미스화 하고, `async`/`await`을 사용하는 것을 고려하세요.

이렇게 하면 AVA가 예외를 catch해, 결과적으로 테스트를 올바르게 만들어 줄 것입니다.

## 비동기 테스트 간 변수 공유

기본적으로 AVA는 테스트들을 동시에(concurrently) 수행합니다.

이것은 테스트가 비동기적이고, 변수들을 공유할 때 문제를 일으킬 수 있습니다.

아래의 인위적인 예를 들여다 보세요:

```js
const test = require('ava');

let count = 0;
const incr = async () => {
	await true;
	count = count + 1;
};

test.beforeEach('reset the count', () => {
	count = 0;
});

test('increment once', async t => {
	await incr();
	t.is(count, 1);
});

test('increment twice', async t => {
	await incr();
	await incr();
	t.is(count, 2);
});
```

동시(concurrent) 테스트를 통해 비동기 테스트를 보다 빠르게 실행할 수 있습니다.

그러나 테스트가 공유 상태(변수)에 의존할 경우, 예기치 않은 테스트 실패를 초래할 수 있습니다.

공유 상태를 피할 수 없는 경우 테스트를 직렬로 실행할 수 있습니다.

```js
const test = require('ava');

let count = 0;
const incr = async () => {
	await true;
	count = count + 1;
};

test.beforeEach('reset the count', () => {
	count = 0;
});

test.serial('increment once', async t => {
	await incr();
	t.is(count, 1);
});

test.serial('increment twice', async t => {
	await incr();
	await incr();
	t.is(count, 2);
});
```

---

당신의 문제가 여기에 나열되어 있지 않나요?

Pull request를 제출하거나, [이 이슈](https://github.com/avajs/ava/issues/404)에 코멘트를 남기세요.
