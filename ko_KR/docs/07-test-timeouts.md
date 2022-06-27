___
**역자주**

이 문서는 [07-test-timeouts.md](https://github.com/avajs/ava/blob/main/docs/07-test-timeouts.md)의 한국어 번역입니다. [이곳](https://github.com/avajs/ava/compare/71404c23302d825095659c70cb9a1b08251697ad...main#diff-0730bb7c2e8f9ea2438b52e419dd86c9)에서 AVA의 master 브랜치와 이 문서의 차이를 확인할 수 있습니다. (만약 차이가 없다면 문서가 최신 버전임을 의미합니다)
___

# Timeout 테스트

[![StackBlitz 예제 코드 보기](https://developer.stackblitz.com/img/open_in_stackblitz.svg)](https://stackblitz.com/github/avajs/ava/tree/main/examples/timeouts?file=test.js&terminal=test&view=editor)

AVA의 timeout는 다른 테스트 프레임워크의 timeout과 다르게 작동합니다.

AVA는 매 테스트 종료 후 타이머를 리셋하며, 지정된 timeout 내에 새 테스트 결과가 수신되지 않은 경우 테스트를 강제로 종료합니다.

이것은 정지된 테스트를 처리하는데 사용될 수 있습니다.

timeout 값은 기본으로 10초로 지정됩니다.

timeout 값을 `--timeout` [command line option](./05-command-line.md) 인자로 넘겨주거나, [configuration](./06-configuration.md)에 설정할 수 있습니다.

이것은 읽기 쉬운(human-readable) 방식으로 지정될 수 있습니다:

```console
npx ava --timeout=10s # 10 초
npx ava --timeout=2m # 2 분
npx ava --timeout=100 # 100 밀리 초
```

### `t.timeout(ms, message?)`

각 테스트에 대해 개별적으로 제한 시간을 설정할 수도 있습니다. timeout은 단언문이 실행될 때마다 재설정됩니다.

단언문이 생성되거나 테스트가 완료되는데 `ms` 이상의 시간이 소요될 경우 테스트가 실패합니다.

```js
test('foo', t => {
	t.timeout(100); // 100 밀리 초
	// 여기에 단언문을 작성하세요.
});
```

timeout에 메시지 문자열을 제공할 수도 있습니다.

이 기능은 당신의 테스트가 아직 완료되지 않았을 수 있는 다른 설정에 의존하는 경우에 유용할 수 있습니다.

```js
test('foo', t => {
	t.timeout(100, 'make sure database has started'); // 100 밀리 초
	// 여기에 단언문을 작성하세요.
});
```
