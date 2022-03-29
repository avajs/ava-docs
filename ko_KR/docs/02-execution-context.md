___
**역자주**

이 문서는 [02-execution-context.md](https://github.com/avajs/ava/blob/main/docs/02-execution-context.md)의 한국어 번역입니다. [이곳](https://github.com/avajs/ava/compare/71404c23302d825095659c70cb9a1b08251697ad...main#diff-0730bb7c2e8f9ea2438b52e419dd86c9)에서 AVA의 master 브랜치와 이 문서의 차이를 확인할 수 있습니다. (만약 차이가 없다면 문서가 최신 버전임을 의미합니다)
___

# 실행 컨텍스트 (`t` 인자)

각 테스트, hook들은 실행 컨텍스트(execution context)와 함께 호출됩니다.

실행 컨텍스트란, 컨벤션으로 `t`라고 이름 지어진 인자입니다.

```js
import test from 'ava';

test('my passing test', t => {
	t.pass();
});
```

각각의 테스트, hook들은 서로 다른 객체를 넘겨 받습니다.

해당 객체는 [단언문](./03-assertions.md)(assertions)을 메서드 및 프로퍼티 형태로 포함하고 있습니다.

해당 메서드와 프로퍼티들은 아래와 같습니다.

## `t.title`

테스트 타이틀.

## `t.context`

hook들 간 공유되는 상태.

## `t.passed`

`test.afterEach()`, `test.afterEach.always()` hook들을 사용할 때, 해당 테스트가 통과(pass) 되었는지를 나타냅니다.

테스트 자체에서 사용될 땐 (teardown 함수들을 포함해) 이것은 단언문(assertions)이 실패하거나, 테스트가 에러를 던지면서 끝나거나, teardown 함수가 에러를 일으킬 때 까지 `true`로 남아 있습니다.

이 값은 다른 hook들에선 아무 의미도 갖지 않습니다.

## `t.log(...values)`

즉시 `stdout`에 출력하는 게 아니라, 문맥에 맞게 테스트 결과와 함께 기록 합니다.

`console.log`처럼 다소 비슷하게 작동하지만, placeholder token은 지원하지 않습니다.

## `t.plan(count)`

몇 개의 단언문들이 테스트에서 실행되어야 하는지 설정합니다.

`count`개의 단언문이 실제로 실행되지 않았다면 테스트가 실패할 것입니다.

[단언문 계획하기](./03-assertions.md#plan-단언문-사용하기)를 참조하세요.

## `t.teardown(fn)`

테스트가 끝나고 실행될 `fn` 함수를 등록합니다.

여러 개의 함수들을 등록할 수 있습니다.

등록된 함수들은 역순으로 실행될 것입니다. 즉, 가장 마지막에 등록된 함수가 가장 먼저 실행됩니다.

비동기 함수를 사용할 수 있습니다: 한 번에 하나의 함수가 처리됩니다.

해당 함수 내부에서 `t` 객체로 단언문을 실행하거나, 추가 함수를 등록할 수 없습니다.

또한 hook에서도 `t.teardown()`을 사용할 수 없습니다.

## `t.timeout(ms)`

milliseconds 단위로, 테스트에 대한 타임아웃을 설정합니다.

해당 테스트는 `ms`가 지난 후 실패하게 됩니다.

각 단언문들이 실행될 때 마다 타임아웃 값(`ms`)은 리셋됩니다.
