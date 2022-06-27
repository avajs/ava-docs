___
**역자주**

이 문서는 [endpoint-testing.md](https://github.com/avajs/ava/blob/main/docs/recipes/endpoint-testing.md)의 한국어 번역입니다. [이곳](https://github.com/avajs/ava/compare/71404c23302d825095659c70cb9a1b08251697ad...main#diff-0730bb7c2e8f9ea2438b52e419dd86c9)에서 AVA의 master 브랜치와 이 문서의 차이를 확인할 수 있습니다. (만약 차이가 없다면 문서가 최신 버전임을 의미합니다)
___

# 엔드 포인트 테스팅 (Endpoint testing)

[![StackBlitz 예제 코드 보기](https://developer.stackblitz.com/img/open_in_stackblitz.svg)](https://stackblitz.com/github/avajs/ava/tree/main/examples/endpoint-testing?file=test.js&terminal=test&view=editor)

AVA는 엔드 포인트를 테스트하기 위한 빌트인 기능을 지원하지 않지만, 원하는 HTTP 클라이언트 (예, [`got`](https://github.com/sindresorhus/got))를 선택해 사용할 수 있습니다.

또한 테스트를 병렬로 실행할 수 있도록 고유한 포트에서 HTTP 서버를 시작해야 합니다.

이를 위해 [`test-listen`](https://github.com/zeit/test-listen) 라이브러리를 추천합니다.

테스트가 동시에(concurrently) 실행되므로 각 테스트에 대해, 또는 적어도 각 테스트 파일들에 대해 새 서버 인스턴스를 만드는 것이 가장 좋습니다.

이것은 `test.before()`, `test.beforeEach()` hook 및 `t.context`를 사용해 달성될 수 있습니다.

`test.before()` hook을 사용하여 서버를 시작할 경우, 테스트가 연속적으로(serially) 실행됨을 보장해야 합니다.

아래의 예제를 확인하세요.:

```js
const http = require('http');
const test = require('ava');
const got = require('got');
const listen = require('test-listen');
const app = require('../app');

test.before(async t => {
	t.context.server = http.createServer(app);
	t.context.prefixUrl = await listen(t.context.server);
});

test.after.always(t => {
	t.context.server.close();
});

test.serial('get /user', async t => {
	const {email} = await got('user', {prefixUrl: t.context.prefixUrl}).json();
	t.is(email, 'ava@rocks.com');
});
```

유용할 수 있는 다른 라이브러리들:

- [`supertest`](https://github.com/visionmedia/supertest)
- [`get-port`](https://github.com/sindresorhus/get-port)
