___
**역자주**

이 문서는 [passing-arguments-to-your-test-files.md](https://github.com/avajs/ava/blob/main/docs/recipes/passing-arguments-to-your-test-files.md)의 한국어 번역입니다. [이곳](https://github.com/avajs/ava/compare/71404c23302d825095659c70cb9a1b08251697ad...main#diff-0730bb7c2e8f9ea2438b52e419dd86c9)에서 AVA의 master 브랜치와 이 문서의 차이를 확인할 수 있습니다. (만약 차이가 없다면 문서가 최신 버전임을 의미합니다)
___

# 테스트 파일들에 인자(arguments) 넘겨주기

테스트 파일에 커맨드 라인 인자를 넘겨줄 수 있습니다.

`--` argument terminator를 사용해, AVA의 인자와 당신의 인자를 분리하도록 하세요.

```js
// test.js
import test from 'ava';

test('argv', t => {
	t.deepEqual(process.argv.slice(2), ['--hello', 'world']);
});
```

```console
$ npx ava -- --hello world
```

만약 `npm test` 스크립트 내부에서, AVA를 호출하려고 하는 경우, 두 개의 `--` argument terminators가 필요합니다.

```json
{
	"scripts": {
		"test": "ava"
	}
}
```

```console
$ npm test -- -- --hello world
```
