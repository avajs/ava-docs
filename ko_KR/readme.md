___
**역주**

이 문서는 [readme.md](https://github.com/sindresorhus/ava/blob/master/readme.md)의 한국어 번역입니다. [이곳](https://github.com/sindresorhus/ava/compare/93af8d8d2cb48fe0d2c4ede3c92964a295f60cb6...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9)에서 AVA의 master 브랜치와 이 문서의 차이를 확인할 수 있습니다. (만약 차이가 없다면 문서가 최신 버전임을 의미합니다)
___

# ![AVA](https://github.com/sindresorhus/ava/blob/master/media/header.png)

> 미래적인 테스트 러너

[![Build Status: Linux](https://travis-ci.org/sindresorhus/ava.svg?branch=master)](https://travis-ci.org/sindresorhus/ava) [![Build status: Windows](https://ci.appveyor.com/api/projects/status/igogxrcmhhm085co/branch/master?svg=true)](https://ci.appveyor.com/project/sindresorhus/ava/branch/master) [![Coverage Status](https://coveralls.io/repos/sindresorhus/ava/badge.svg?branch=master&service=github)](https://coveralls.io/github/sindresorhus/ava?branch=master) [![Gitter](https://badges.gitter.im/join_chat.svg)](https://gitter.im/sindresorhus/ava)

비록 JavaScript가 싱글-스레드 기반으로 작동할지라도, Node.js에서의 입출력은 비동기 생태계를 통해 병렬 처리가 가능합니다. AVA는 무거운 입출력 작업에 특히 유용한 이 장점을 적극적으로 활용하여 테스트를 동시에 처리합니다. 또한, 테스트 파일들은 분리된 프로세스처럼 병렬로 실행되므로 더 좋은 성능과 각 테스트 파일에 대한 고립된 환경을 제공합니다. Mocha에서 AVA로 [이전](https://github.com/sindresorhus/pageres/commit/663be15acb3dd2eb0f71b1956ef28c2cd3fdeed0)한 Pageres는 테스트 시간을 31초에서 11초로 줄였습니다. 테스트를 동시에 실행하는 능력을 갖추는 것은 더 작은 테스트를 작성하도록 유도합니다. 이 말은 테스트가 더 이상 전역 상태나 다른 테스트의 상태에 의존하지 않는다는 것입니다. 정말 대단하지 않나요?

*기여하는 방법에 대해 찾아보고 있다면 (이슈/PR/etc), 우리의 [기여 가이드](contributing.md)를 확인하세요.*

[AVA 트위터 계정](https://twitter.com/ava__js)을 팔로우하면 업데이트에 대한 소식을 받아볼 수 있습니다.

전체 번역 문서들: [Español](https://github.com/sindresorhus/ava-docs/blob/master/es_ES/readme.md), [Français](https://github.com/sindresorhus/ava-docs/blob/master/fr_FR/readme.md), [Italiano](https://github.com/sindresorhus/ava-docs/blob/master/it_IT/readme.md), [日本語](https://github.com/sindresorhus/ava-docs/blob/master/ja_JP/readme.md), [Português](https://github.com/sindresorhus/ava-docs/blob/master/pt_BR/readme.md), [Русский](https://github.com/sindresorhus/ava-docs/blob/master/ru_RU/readme.md), [简体中文](https://github.com/sindresorhus/ava-docs/blob/master/zh_CN/readme.md), [한국어](https://github.com/sindresorhus/ava-docs/blob/master/ko_KR/readme.md)

## 목차

- [사용법](#사용법)
- [CLI 사용법](#cli-사용법)
- [구성](#구성)
- [참조 문서](#참조-문서)
- [API](#api)
- [가정](#가정)
- [도움말](#도움말)
- [FAQ](#faq)
- [레시피](#레시피)
- [지원](#지원)
- [관련된 프로젝트](#관련된-프로젝트)
- [링크](#링크)
- [팀](#팀)

## 왜 AVA인가요?

- 작고 빠릅니다
- 간단한 테스트 문법
- 테스트를 동시적으로 실행
- 작은 테스트를 작성하도록 유도
- 내재적인 전역 변수 없음
- [각 테스트 파일마다 고립된 환경](#process-isolation)
- [ES2015로 테스트 작성](#es2015-support)
- [Promise 지원](#promise-support)
- [생성기 함수 지원](#generator-function-support)
- [비동기 함수 지원](#async-function-support)
- [Observable 지원](#observable-support)
- [강화된 가정 메시지](#enhanced-assertion-messages)
- [선택적인 TAP 출력](#optional-tap-output)
- [깨끗한 스텍 기록](#clean-stack-traces)

## 테스트 문법

```js
import test from 'ava';

test(t => {
	t.deepEqual([1, 2], [1, 2]);
});
```

## 사용법

### AVA를 프로젝트에 추가하기

AVA를 전역에 설치한 후 `--init` 인수와 함께 실행하면 `package.json`에 AVA를 추가할 수 있습니다:

```console
$ npm install --global ava
$ ava --init
```

```json
{
  "name": "awesome-package",
  "scripts": {
    "test": "ava"
  },
  "devDependencies": {
    "ava": "^0.11.0"
  }
}
```

`--init` 뒤에 전달된 인수는 `package.json`에 추가됩니다.

#### 수동 설치

물론 AVA를 직접 설치할 수도 있습니다:

```console
$ npm install --save-dev ava
```

`package.json`의 `test` 스크립트에서 `ava`를 사용하도록 설정해야 합니다.
(위를 참고하세요)

### 테스트 파일 만들기

프로젝트 루트 디렉터리에 `test.js` 파일을 하나 생성합니다:

```js
import test from 'ava';

test('foo', t => {
	t.pass();
});

test('bar', async t => {
	const bar = Promise.resolve('bar');

	t.is(await bar, 'bar');
});
```

<img src="https://github.com/sindresorhus/ava/blob/master/screenshot.png" width="150" align="right">

### 실행하기

```console
$ npm test
```

### 감시 모드로 실행하기

```console
$ npm test -- --watch
```

AVA는 지능형 감시 모드와 함께 제공됩니다. [이 레시피에 대해 자세히 알아보기](docs/recipes/watch-mode.md).

## CLI 사용법

![](https://github.com/sindresorhus/ava/raw/master/screenshot-mini-reporter.gif)

```console
$ ava --help

  Usage
    ava [<file|directory|glob> ...]

  Options
    --init           Add AVA to your project
    --fail-fast      Stop after first test failure
    --serial, -s     Run tests serially
    --require, -r    Module to preload (Can be repeated)
    --tap, -t        Generate TAP output
    --verbose, -v    Enable verbose output
    --no-cache       Disable the transpiler cache
    --match, -m      Only run tests with matching title (Can be repeated)
    --watch, -w      Re-run tests when tests and source files change
    --source, -S     Pattern to match source files so tests can be re-run (Can be repeated)
    --timeout, -T    Set global timeout

  Examples
    ava
    ava test.js test2.js
    ava test-*.js
    ava test
    ava --init
    ava --init foo.js

  Default patterns when no arguments:
  test.js test-*.js test/**/*.js **/__tests__/**/*.js **/*.test.js
```

*참고로 CLI는 가능한 경우 로컬에 설치된 AVA를 사용하며, 전역에 설치한 경우에도 같습니다.*

디렉터리는 재귀적이며, 동시에 모든 `*.js` 파일을 테스트 파일로 인식합니다. 디렉터리의 이름이 `fixtures`, `helpers` 그리고 `node_modules`인 경우 *항상* 무시됩니다. 따라서 파일 이름을 `_`로 시작하도록 만들면 테스트 파일과 같은 디렉터리를 사용함으로써 헬퍼를 테스트 파일과 같은 디렉터리에 배치할 수 있습니다.

`npm test`를 사용할 때, `npm test test2.js`와 같은 위치 인수를 전달할 수 있으나, 플래그가 `npm test -- --verbose`와 같이 전달되도록 해야 합니다.

## 구성

모든 CLI 옵션은 `package.json`의 `ava` 섹션에서 구성할 수 있습니다. 이렇게 하면 `ava`의 기본 동작을 변경할 수 있으며 CLI에서 매번 똑같은 옵션을 치지 않아도 됩니다.

```json
{
  "ava": {
    "files": [
      "my-test-folder/*.js",
      "!**/not-this-file.js"
    ],
    "source": [
      "**/*.{js,jsx}",
      "!dist/**/*"
    ],
    "match": [
      "*oo",
      "!foo"
    ],
    "failFast": true,
    "tap": true,
    "require": [
      "babel-register"
    ],
    "babel": "inherit"
  }
}
```

CLI로 전달되는 인수는 언제나 `package.json`의 구성 옵션보다 높은 우선순위를 가집니다.

`babel` 옵션에 대해선 [ES2015 지원](#es2015-지원) 섹션을 확인하세요.

## 참조 문서

테스트는 동시적으로 실행됩니다. 동기와 비동기 테스트를 지정할 수 있습니다. 테스트는 promise나 [observable](https://github.com/zenparsing/zen-observable)을 반환하지 않는 이상 동기 코드로 작성되어야 합니다.

우리는 [비동기 함수](#비동기-함수-지원) 함수를 사용하는 것을 *매우* 추천합니다. 이는 코드를 간결하게 만들고 읽기 좋게 하며, 자체적으로 promise를 반환하므로 따로 신경쓰지 않아도 됩니다.

만약 promise 또는 observable을 사용할 수 없다면, "콜백 모드"를 활성화 하여 `test.cb([title], fn)`와 같이 테스트를 정의할 수 있습니다. 이렇게 선언한 테스트는 **반드시** 직접 `t.end()`를 호출해 주어야 합니다. 이 모드는 콜백-스타일 API 테스팅을 주 대상으로 합니다.

반드시 모든 테스트를 동기 코드로 작성하여야 합니다. 내부에서 `setTimeout`, `setImmediate`, 등의 메서드는 사용할 수 없습니다.

테스트 파일은 자체의 현재 디렉터리에서 실행되므로 [`process.cwd()`](https://nodejs.org/api/process.html#process_process_cwd)는 언제나 [`__dirname`](https://nodejs.org/api/globals.html#globals_dirname)와 같습니다. 따라서 `path.join(__dirname, 'relative/path')`와 같이 쓸 필요 없이 언제나 상대경로를 사용할 수 있습니다.

### 테스트 만들기

테스트를 만드려면 AVA에서 들여오기한 `test` 함수를 호출해야 합니다. 이 함수는 선택적인 제목과 구현 함수를 인자로 받습니다. 여기서 전달된 구현 함수는 테스트가 실행될 때 호출됩니다. 이 함수는 [실행 객체](#t)를 전달하며 이게 첫 번째이자 유일한 인수입니다. 규칙에 의해 이 인자의 이름은 `t`가 되었습니다.

```js
import test from 'ava';

test('my passing test', t => {
	t.pass();
});
```

#### 제목

제목은 선택적이므로 다음과 같이 쓸 수 있습니다:

```js
test(t => {
	t.pass();
});
```

한 개 이상의 테스트를 가지고 있으면 테스트 제목을 입력하는 것을 추천합니다.

테스트의 제목을 입력하지 않고, 구현 함수가 이름있는 함수인 경우, 함수의 이름이 테스트 제목으로 사용됩니다:

```js
test(function name(t) {
	t.pass();
});
```

### 가정 계획하기

가정 계획은 테스트가 지정한 개수 만큼의 가정이 확실하게 실행됐을 때만 통과시키도록 합니다. 이 기능은 테스트가 너무 빨리 종료될 때의 경우를 잡을 때 도움이 됩니다. 또한, 이 기능을 사용할 경우 가정이 너무 많아도 테스트가 실패되며, 가정이 콜백이나 루프에 있을 때 유용하게 활용할 수 있습니다.

참고로, [`tap`](https://www.npmjs.com/package/tap)과 [`tape`](https://www.npmjs.com/package/tape)와는 달리, AVA는 가정의 개수가 계획된 가정의 개수에 도달해도 자동적으로 테스트를 *끝내지* 않습니다.

다음 예시는 테스트가 통과된 결과를 보여줍니다:

```js
test(t => {
	t.plan(1);

	return Promise.resolve(3).then(n => {
		t.is(n, 3);
	});
});

test.cb(t => {
	t.plan(1);

	someAsyncFunction(() => {
		t.pass();
		t.end();
	});
});
```

다음 예시는 테스트가 통과되지 않은 결과를 보여줍니다:

```js
test(t => {
	t.plan(2);

	for (let i = 0; i < 3; i++) {
		t.true(i < 3);
	}
}); // 실패, 3개의 가정이 실행됐으며 너무 많습니다

test(t => {
	t.plan(1);

	someAsyncFunction(() => {
		t.pass();
	});
}); // 실패, 가정이 실행되기 전에 동기적인 테스트가 먼저 끝나버렸습니다
```

### 테스트를 직렬로 실행하기

기본적으로 테스트는 동시에 실행되며 아주 대단합니다. 그래도 때로는 동시에 실행되지 않는 테스트를 작성해야 할 때가 있습니다.

이러한 특수한 경우엔 `.serial` 한정자를 사용하면 됩니다. 이는 테스트를 동시에 실행되기 *전에* 순차적으로 실행되도록 강제합니다.

```js
test.serial(t => {
	t.pass();
});
```

참고로 이는 특정한 테스트 파일 안에서만 작동합니다. [`--serial` CLI 플래그](#cli-사용법)를 전달하지 않는 이상 AVA는 여전히 테스트 파일들을 동시에 실행할 것입니다.

### 특정 테스트 실행하기

개발 도중엔 몇몇 특정한 테스트만 실행하는 것이 도움될 수 있습니다. 이는 `.only` 한정자를 통해 구현할 수 있습니다:

```js
test('will not be run', t => {
	t.fail();
});

test.only('will be run', t => {
	t.pass();
});
```
`.only`는 모든 테스트 파일에 걸쳐 적용되며, 만약 한 개의 파일에서 사용하면 다른 파일의 테스트가 실행되지 않습니다.

### 제목이 일치하는 테스트를 실행하기

`--match` 플래그를 사용하면 지정한 제목에 일치하는 테스트만 실행할 수 있습니다. 여기서 사용하는 패턴은 간단한 와일드카드 패턴으로 이루어집니다. 패턴은 대소문자를 구분합니다. 더 자세한 내용은 [`matcher`](https://github.com/sindresorhus/matcher)를 확인하세요.

`foo`로 끝나는 제목을 일치시킵니다:

```console
$ ava --match='*foo'
```

`foo`로 시작하는 제목을 일치시킵니다:

```console
$ ava --match='foo*'
```

`foo`를 포함하고 있는 제목을 일치시킵니다:

```console
$ ava --match='*foo*'
```

`foo`와 *정확하게* 일치하는 제목을 일치시킵니다 (알파벳은 대소문자 구분합니다):

```console
$ ava --match='foo'
```

`foo`를 포함하고 있지 않은 제목을 일치시킵니다:

```console
$ ava --match='!*foo*'
```

`foo`로 시작하고 `bar`로 끝나는 제목을 일치시킵니다:

```console
$ ava --match='foo*bar'
```

`foo` 또는 `bar`로 시작하는 제목을 일치시킵니다:

```console
$ ava --match='foo*' --match='*bar'
```

참고로 일치 패턴은 `.only` 수정자보다 높은 우선순위를 가집니다. 정확히 일치하는 제목의 테스트만 실행합니다. `--match`를 사용할 땐 제목이 없는 테스트와 함수의 이름으로부터 가져온 제목을 가진 테스트가 모두 무시됩니다.

다음 예시는 AVA에서 `*oo*` 패턴과 함께 다음 테스트를 실행시켰을 때의 결과입니다:

```js
test('foo will run', t => {
	t.pass();
});

test('moo will also run', t => {
	t.pass();
});

test.only('boo will run but not exclusively', t => {
	t.pass();
});

// 제목이 없으므로 실행되지 않습니다
test(function (t) {
	t.fail();
});

// 명시적인 제목이 없으므로 실행되지 않습니다
test(function foo(t) {
	t.fail();
});
```

### 테스트 건너뛰기

가끔 실패하는 테스트는 고치기 어려울 때가 있습니다. 이럴 땐 `.skip` 한정자를 사용하여 AVA에게 이 테스트를 넘기도록 알릴 수 있습니다. 테스트가 실행되진 않지만, 여전히 출력엔 계속 표시됩니다. ('테스트 건너뜀'과 같이)

```js
test.skip('will not be run', t => {
	t.fail();
});
```

구현 함수를 반드시 지정해야 합니다.

### 테스트 비워두기 ("todo")

`.todo` 한정자를 사용하여 테스트를 계획만 해두고 나중에 작성할 수 있습니다. 마치 테스트를 건너뛰고 대신 이 placeholder가 출력됩니다. 이 한정자는 단지 제목 인수만을 요구하며 구현 함수를 지정할 수 없습니다.

```js
test.todo('will think about writing this later');
```

### 이전과 이후 훅

AVA는 테스트를 실행하기 이전과 테스트 이후에 훅을 등록할 수 있게 해줍니다. 이 기능은 코드를 설정 또는 해제 코드를 실행할 수 있도록 합니다.

`test.before()`는 테스트 파일의 첫 테스트가 실행되기 이전에 실행될 훅을 등록합니다. 비슷하게 `test.after()`는 마지막 테스트가 끝난 이후에 실행될 훅을 등록합니다.

`test.beforeEach()`는 테스트 파일의 각 테스트가 실행되기 이전에 실행될 훅을 등록합니다. 비슷하게 `test.afterEach()`는 각 테스트가 끝난 이후에 실행될 훅을 등록합니다.

`test()`와 같이 이 메서드들은 선택적인 제목과 콜백 함수를 가질 수 있습니다. 제목은 훅 실행에 실패했을 경우 표시됩니다. 콜백은 [실행 객체](#t)와 같이 호출됩니다.

`before` 훅은 `beforeEach` 훅 이전에 실행됩니다. `afterEach` 훅은 `after` 훅 이전에 실행됩니다. 이러한 범주 내에서 훅은 정의된 순서대로 실행됩니다.

```js
test.before(t => {
	// 모든 테스트 이전에 실행됩니다
});

test.before(t => {
	// 위가 실행된 후 실행됩니다. 하지만 여전히 테스트 이전에 실행됩니다
});

test.after('cleanup', t => {
	// 모든 테스트의 이후에 실행됩니다
});

test.beforeEach(t => {
	// 각 테스트 이전에 실행됩니다
});

test.afterEach(t => {
	// 각 테스트 이후에 실행됩니다
});

test(t => {
	// 정상적인 테스트
});
```

훅은 테스트와 같이 동기 또는 비동기가 될 수 있습니다. 훅을 비동기로 만드려면 promise나 observable을 반환하면 되며 비동기 함수와 `test.cb.before()`, `test.cb.beforeEach()`를 통해 콜백 모드를 활성화 시켜도 됩니다.

```js
test.before(async t => {
	await promiseFn();
});

test.after(t => {
	return new Promise(/* ... */);
});

test.cb.beforeEach(t => {
	setTimeout(t.end);
});

test.afterEach.cb(t => {
	setTimeout(t.end);
});
```

참고로 `beforeEach`와 `afterEach` 그냥 테스트의 이전과 이후에 각각 실행되며 기본적으로 테스트를 동시에 실행시킵니다. 만약 각 테스트를 위해 전역 상태를 설정해야 할 필요가 있는 경우 ([예시](https://github.com/sindresorhus/ava/issues/560)와 같이 `console.log`를 감시한다거나), 테스트가 확실하게 [직렬화로 실행](#테스트를-직렬로-실행하기)하는지 확인해야 합니다.

참고로 AVA는 각 테스트 파일을 분리된 프로세스에서 실행합니다. 따라서 `이후`-훅이 바로 프로세스가 종료되기 전에 실행되는 이상 따로 전역 상태를 정리하지 않아도 됩니다.

`beforeEach` & `afterEach` 훅은 다음과 같이 컨텍스트를 테스트와 공유할 수 있습니다:

```js
test.beforeEach(t => {
	t.context.data = generateUniqueData();
});

test(t => {
	t.is(t.context.data + 'bar', 'foobar');
});
```

기본적으로 `t.context`는 객체이지만 다시 할당할 수 있습니다:

```js
test.beforeEach(t => {
	t.context = 'unicorn';
});

test(t => {
	t.is(t.context, 'unicorn');
});
```

`before`와 `after` 훅 사이의 컨텍스트 공유는 할 수 *없습니다*.

### 테스트 한정자 체이닝하기

`.serial`, `.only` 그리고 `.skip` 한정자는 순서에 상관없이, `test`, `before`, `after`, `beforeEach` 그리고 `afterEach`와 같이 사용할 수 있습니다. 예시는 다음과 같습니다:

```js
test.before.skip(...);
test.skip.after(...);
test.serial.only(...);
test.only.serial(...);
```

이 말은 임시적으로 `.skip` 또는 `.only`를 테스트의 끝에 추가하거나 훅 선언에서 별 다른 수정 없이 사용할 수 있다는 말입니다.

### 커스텀 가정

내장에서 제공하는 가정 라이브러리 대신, 또는 추가적으로 가정 실패시 예외를 던지는것을 지원한다면, 어느 가정 라이브러리라도 사용할 수 있습니다.

이는 [빌트-인 가정](#가정)을 사용하는 것과는 달리 좋은 경험을 제공하지 못하며 [가정 계획](#가정-계획하기)을 사용할 수 없습니다. ([이슈 #25](https://github.com/sindresorhus/ava/issues/25)를 참고하세요)

```js
import assert from 'assert';

test(t => {
	assert(true);
});
```

### ES2015 지원

AVA는 [Babel 6](https://babeljs.io)를 통한 ES2015를 내부적으로 기본 지원합니다. 그저 테스트 코드를 ES2015로 작성하면 됩니다. 다른 추가 작업은 필요하지 않습니다. 또한 어떠한 Babel 버전도 프로젝트 내에서 사용할 수 있습니다. 내부에서 자체적으로 [`es2015`](https://babeljs.io/docs/plugins/preset-es2015/)와 [`stage-2`](https://babeljs.io/docs/plugins/preset-stage-2/) 프리셋을 사용함과 동시에 [`espower`](https://github.com/power-assert-js/babel-plugin-espower)와 [`transform-runtime`](https://babeljs.io/docs/plugins/transform-runtime/) 플러그인을 사용합니다.

AVA에서 사용하는 Babel 구성은 다음과 같습니다:

```json
{
  "presets": [
    "es2015",
    "stage-2"
  ],
  "plugins": [
    "espower",
    "transform-runtime"
  ]
}
```

AVA의 [`package.json` 구성](#구성)에서 `babel` 옵션을 통해 AVA 테스트 파일의 트랜스파일 동작을 커스터마이즈 할 수 있습니다. 예를 들어 프리셋을 덮어쓰려면 다음과 같이 설정할 수 있습니다:

```json
{
	"ava": {
		 "babel": {
			 "presets": [
					"es2015",
					"stage-0",
					"react"
			 ]
		 }
	},
}
```

또한 특별한 `"inherit"` 키워드를 사용할 수 있습니다. 이 키워드는 AVA가 프로젝트 내의 [`.babelrc` 또는 `package.json` 파일](https://babeljs.io/docs/usage/babelrc/)을 사용하도록 구성을 연기합니다. 이 방법을 사용하면 AVA를 위해 추가적인 설정을 할 필요 없이 소스 파일에 대한 구성을 그대로 사용하여 테스트 파일을 트랜스파일 할 수 있습니다:

 ```json
{
	"babel": {
		"presets": [
			"es2015",
			"stage-0",
			"react"
		]
	},
	"ava": {
		"babel": "inherit"
	},
}
```
더 많은 예시와 자세한 구성 옵션에 대한 설명은 AVA의 [`.babelrc` 레시피](docs/recipes/babelrc.md)를 참고하세요.

참고로 AVA는 구성에 상관없이 *언제나* [내부적으로 몇 가지 플러그인](docs/recipes/babelrc.md#notes)을 적용합니다. 하지만 실제 코드에 영향을 줄 가능성은 적습니다.

### TypeScript 지원

AVA는 TypeScript의 타이핑을 포함하고 있습니다. 하지만 직접적인 설정이 필요합니다. `tsconfig.json` 파일에서 `module`을 `commonjs`로 설정하면, TypeScript는 자동으로 AVA에 대한 타입 정의를 찾을 것입니다. Promise와 비동기 함수를 사용하려면 `target`을 `es2015`로 설정해야 합니다.

### 들여온 모듈 트랜스파일하기

현재로써 AVA는 지정한 테스트 파일만 트랜스파일합니다. *따라서 테스트 밖에서 `import`로 들여오는 모듈은 트랜스파일하지 않습니다.* 이러한 상황은 예상치 못한 상황이겠지만, 해결 방법이 있습니다.

만약 Babel을 사용한다면 [require 훅](https://babeljs.io/docs/usage/require/)을 사용하여 들여오는 모듈을 즉석에서 트랜스파일 할 수 있습니다. AVA를 `--require babel-register` 옵션 ([CLI 사용법](#cli-사용법) 참고)과 함께 실행하거나 [`package.json`에서 설정](#구성)할 수 있습니다.

또한 별도의 프로세스에서 모듈을 트랜스파일할 수도 있으며 테스트에서 소스를 가져오는 대신 트랜스파일된 파일을 참조할 수도 있습니다.

### Promise 지원

테스트에서 promise를 반환하는 경우 promise가 resolve될 때 자동으로 테스트가 끝나므로 따로 명시적으로 끝낼 필요가 없습니다.

```js
test(t => {
	return somePromise().then(result => {
		t.is(result, 'unicorn');
	});
});
```

### 생성기 함수 지원

AVA는 내부적으로 [생성기 함수](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/function*)를 지원합니다.

```js
test(function * (t) {
	const value = yield generatorFn();
	t.true(value);
});
```

### 비동기 함수 지원

AVA는 내부적으로 [비동기 함수](https://tc39.github.io/ecmascript-asyncawait/) *(async/await)* 를 지원합니다.

```js
test(async function (t) {
	const value = await promiseFn();
	t.true(value);
});

// 비동기 화살표 함수
test(async t => {
	const value = await promiseFn();
	t.true(value);
});
```

### Observable 지원

AVA는 내부적으로 [observables](https://github.com/zenparsing/es-observable)을 지원합니다. 만약 테스트에서 observable을 반환한다면, AVA는 테스트가 끝나기 전에 자동으로 소멸시키고 완료합니다.

*"callback 모드"나 `t.end()`를 호출할 필요가 없습니다.*

```js
test(t => {
	t.plan(3);
	return Observable.of(1, 2, 3, 4, 5, 6)
		.filter(n => {
			// 짝수 숫자만
			return n % 2 === 0;
		})
		.map(() => t.pass());
});
```

### 콜백 지원

AVA는 node-스타일 error-first 콜백 API를 사용하는 마지막 콜백에서 `t.end`를 호출하여 작업을 완료하는 기능을 지원합니다. AVA는 `t.end`로 전달되는 첫 번째 인수 값이 truthy 값인 경우 에러를 발생시킵니다. 참고로 `t.end`는 "callback mode"의 활성화를 요구하며, 이는 `test.cb` 체인을 사용함으로써 활성화 할 수 있습니다.

```js
test.cb(t => {
	// t.end가 자동으로 첫 번째 인수를 통해 에러를 확인합니다
	fs.readFile('data.txt', t.end);
});
```

### 선택적인 TAP 출력

AVA는 `--tap` 옵션과 [TAP 리포터](https://github.com/sindresorhus/awesome-tap#reporters)를 통해 TAP 출력을 생성할 수 있습니다.

```console
$ ava --tap | tap-nyan
```

<img src="https://github.com/sindresorhus/ava/blob/master/media/tap-output.png" width="398">

참고로 TAP 리포터는 [감시 모드](#감시-모드로-실행하기)를 사용할 땐 사용할 수 없습니다.

### 깨끗한 스택 기록

AVA는 스택 출력에서 관련이 없는 줄을 자동으로 삭제하며, 더 빠르게 에러의 원점을 포착할 수 있도록 도와줍니다.

<img src="https://github.com/sindresorhus/ava/raw/master/media/stack-traces.png" width="398">

### 전역 timeout

전역 timeout은 `--timeout`옵션을 통해 설정할 수 있습니다.
AVA에서의 timeout은 다른 테스트 프레임워크랑은 달리 약간 다르게 동작합니다.
AVA는 각 테스트가 끝난 후 타이머를 리셋하며, 지정한 timeout내에 새로운 테스트 결과가 없을 경우 강제로 테스트를 종료시킵니다.

Timeout은 사람이 읽기 좋은 포맷으로 설정할 수 있습니다:

```
$ ava --timeout=10s # 10 seconds
$ ava --timeout=2m # 2 minutes
$ ava --timeout=100 # 100 milliseconds
```

## API

### `test([title], implementation)`
### `test.serial([title], implementation)`
### `test.cb([title], implementation)`
### `test.only([title], implementation)`
### `test.skip([title], implementation)`
### `test.todo(title)`
### `test.before([title], implementation)`
### `test.after([title], implementation)`
### `test.beforeEach([title], implementation)`
### `test.afterEach([title], implementation)`

#### `title`

Type: `string`

테스트의 제목.

#### `implementation(t)`

Type: `function`

테스트의 내용이 들어갈 구현 함수.

##### `t`

Type: `object`

각 테스트의 실행 객체입니다. 각각의 테스트 구현은 서로 다른 객체를 받습니다. 이 객체는 [가정](#가정) API를 가지고 있고 `.plan(count)`과 `.end()` 같은 메서드도 포함하고 있습니다. `t.context`는 `beforeEach` 훅에서 온 공유 상태를 담을 수 있습니다.

###### `t.plan(count)`

몇 개의 가정이 테스트에서 실행될지를 계획합니다. 지정한 개수와 계획한 가정이 정확히 일치하지 않을 경우 테스트는 실패합니다. 자세한 내용은 [가정 계획하기](#가정-계획하기)를 참고하세요.

###### `t.end()`

테스트를 끝냅니다. `test.cb()`에서 실행했을 때만 작동합니다.

## 가정

가정은 [실행 객체](#t)에 혼합되어 있으며 각 테스트 구현에 제공됩니다:

```js
test(t => {
	t.truthy('unicorn'); // assertion
});
```

만약 한 개의 테스트에서 여러 개의 가정이 실패하면, AVA는 가장 *첫 번째* 에러만 표시합니다.

### `.pass([message])`

가정을 통과시킵니다.

### `.fail([message])`

가정을 실패시킵니다.

### `.truthy(value, [message])`

`value`가 truthy 값일 경우 가정을 통과시킵니다.

### `.falsy(value, [message])`

`value`가 falsy 값일 경우 가정을 통과시킵니다.

### `.true(value, [message])`

`value`가 `true`일 경우 가정을 통과시킵니다.

### `.false(value, [message])`

`value`가 `false`일 경우 가정을 통과시킵니다.

### `.is(value, expected, [message])`

`value`가 `expected`에 일치하면 가정을 통과시킵니다.

### `.not(value, expected, [message])`

`value`가 `expected`에 일치하지 않으면 가정을 통과시킵니다.

### `.deepEqual(value, expected, [message])`

`value`가 `expected`에 완벽하게 일치하면 가정을 통과시킵니다.

### `.notDeepEqual(value, expected, [message])`

`value`가 `expected`에 완벽하게 일치하지 않으면 가정을 통과시킵니다.

### `.throws(function|promise, [error, [message]])`

`function`이 에러를 throw하거나, `promise`가 에러와 함께 reject되면 가정을 통과시킵니다.

`error`는 생성자, 정규식, 에러 메시지, 유효성 검사 함수가 될 수 있습니다.

`function`에서 throw된 에러를 반환하거나 `promise`의 rejection 이유를 반환합니다.

### `.notThrows(function|promise, [message])`

`function`이 `error`를 throw하지 않거나 또는 `promise`가 resolve되었을 때 가정을 통과시킵니다.

### `.regex(contents, regex, [message])`

`contents`가 `regex`에 일치할 때 가정을 통과시킵니다.

### `.ifError(error, [message])`

`error`가 falsy 값일 경우 가정을 통과시킵니다.

### 가정 건너뛰기

모든 가정은 `skip` 한정자를 통해 건너뛸 수 있습니다. 건너뛰어진 가정은 여전히 카운트되며, 가정 계획에서 지정한 가정 개수를 변경할 필요가 없습니다.

```js
test(t => {
	t.plan(2);
	t.skip.is(foo(), 5); // 가정이 건너뛰어졌을 땐 계획의 개수를 수정할 필요가 없습니다
	t.is(1, 1);
});
```

### 강화된 가정 메시지

AVA는 내부적으로 [`power-assert`](https://github.com/power-assert-js/power-assert)를 포함하여 제공되며, 더 많은 가정 설명을 제공합니다. 이는 테스트를 읽고 코드로부터 더 많은 정보를 추론하려 시도합니다.

다음 예시를 봅시다, Node의 표준 [`assert` 라이브러리](https://nodejs.org/api/assert.html)를 사용합니다:

```js
const a = /foo/;
const b = 'bar';
const c = 'baz';
require('assert').ok(a.test(b) || b === c);
```

만약 Node REPL에 붙여넣고 실행하면 다음의 결과를 반환합니다:

```
AssertionError: false == true
```

AVA에선, 다음을 테스트하면:

```js
test(t => {
	const a = /foo/;
	const b = 'bar';
	const c = 'baz';
	t.true(a.test(b) || b === c);
});
```

다음의 결과가 반환됩니다:

```
t.true(a.test(b) || b === c)
       |      |     |     |
       |      "bar" "bar" "baz"
       false
```

## 프로세스 고립

각 테스트파일은 분리된 Node.js 프로세스에서 실행됩니다. 이것은 다른 파일에 영향을 주지 않고 전역 상태 또는 빌트-인을 변경할 수 있도록 합니다. 또한 이는 현대의 멀티-코어 프로세서에서 아주 좋은 성능을 발휘하며, 여러 테스트 파일을 병행으로 실행할 수 있도록 합니다.

## 도움말

### 임시 파일

테스트를 동시에 실행하는 것은 약간 모험이 될 수 있습니다. 파일 IO 작업은 예시 중 하나입니다.

보통, 직렬 테스트는 현재 테스트 경로에 임시 폴더를 하나 생성하고 테스트가 끝나면 그 폴더를 지웁니다. 이는 테스트를 동시에 실행할 때 다른 테스트와 충돌이 일어나는 등의 이유로 인해 작동하지 않을 것입니다. 가장 알맞은 방법은 각 테스트에서 새로운 임시 경로를 생성하여 사용하는 것입니다. [`tempfile`](https://github.com/sindresorhus/tempfile)과 [`temp-write`](https://github.com/sindresorhus/temp-write) 모듈이 도움이 될 것입니다.

### 디버깅

AVA는 기본적으로 테스트를 동시에 실행하며, 디버깅할 때 최적의 환경을 제공합니다. 대신에, 테스트를 직렬로 실행하려면 `--serial` 옵션을 사용하면 됩니다:

```console
$ ava --serial
```

### 코드 커버리지

AVA는 [테스트 파일을 spawn](#프로세스-고립)하기 때문에 [`istanbul`](https://github.com/gotwarlost/istanbul)을 코드 커버리지로 사용할 수 없습니다. 대신에 [`nyc`](https://github.com/bcoe/nyc)를 사용할 수 있으며, 사실상 서브 프로세스를 지원하는 `istanbul`입니다.

버전 `5.0.0`부터 트랜스파일에 관계없이 실제 코드의 커버리지를 보고하기 위해 소스 맵을 사용합니다. 테스트 대상 코드가 인라인 소스 맵 또는 소스 맵 파일에 대한 참조 중 하나를 포함하는지 확인하세요. `babel/register`를 사용하고 있다면 `.babelrc`에서 `sourceMaps` 옵션을 `inline` 으로 설정할 수 있습니다.

## FAQ

### 왜 `mocha`, `tape`, `tap`을 안 쓰나요?

Mocha는 기본적인 인터페이스로써 `describe`와 `it` 같은 암시적인 전역 변수를 사용하도록 요구합니다. (많은 사람이 사용합니다) 이는 좋지 않은 것을 고집하며 프로세스 고립 없이 테스트를 직렬로 실행하므로 느립니다.

Tape와 tap은 상당히 괜찮습니다. AVA는 이들의 문법에서 많은 영감을 받았습니다. 하지만 여전히 테스트를 직렬로 실행합니다. 이들의 기본 [TAP](https://testanything.org) 출력은 사용자 친화적이지 않고 이러한 이유로 결국 언제나 외부 tap 리포터를 사용해야 합니다.

이와 대조하여 AVA는 좋은 것을 고집하며 각 테스트 파일에 분리된 프로세스와 함께 테스트를 동시에 실행합니다. 또한 기본 리포터는 보기 좋으며, 여전히 AVA는 CLI 플래그를 통해 TAP 출력을 지원합니다.

### 커스텀 리포터는 어떻게 사용하나요?

AVA는 TAP 포맷을 지원하며 따라서 모든 [TAP 리포터](https://github.com/sindresorhus/awesome-tap#reporters)와 호환 가능합니다. [`--tap` 플래그](#선택적인-tap-출력)를 사용하여 TAP 출력을 활성화 하세요.

### 이 프로젝트 이름은 어떻게 쓰고 발음이 어떻게 되나요?

AVA입니다. Ava나 ava가 아닙니다. [`/ˈeɪvə/` ay-və](https://github.com/sindresorhus/ava/blob/master/media/pronunciation.m4a?raw=true)로 발음됩니다.

### 헤더의 배경은 무엇인가요?

[안드로메다 은하](https://ko.wikipedia.org/wiki/%EC%95%88%EB%93%9C%EB%A1%9C%EB%A9%94%EB%8B%A4_%EC%9D%80%ED%95%98)입니다.

### 동시성과 병행성은 무슨 차이가 있나요?

[동시성은 병행성이 아닙니다. 동시성은 병행성이 가능하도록 만듭니다.](https://stackoverflow.com/q/1050222)

## 레시피

- [코드 커버리지](docs/recipes/code-coverage.md)
- [감시 모드](docs/recipes/watch-mode.md)
- [엔드 포인트 테스팅](docs/recipes/endpoint-testing.md)
- [`t.plan()`를 언제 사용할까](docs/recipes/when-to-use-plan.md)
- [브라우저 테스팅](docs/recipes/browser-testing.md)
- [TypeScript](docs/recipes/typescript.md)
- [Babel 구성](docs/recipes/babelrc.md)

## 지원

- [Stack Overflow](https://stackoverflow.com/questions/tagged/ava)
- [Gitter 채팅](https://gitter.im/sindresorhus/ava)
- [Twitter](https://twitter.com/ava__js)

## 관련된 프로젝트

- [sublime-ava](https://github.com/sindresorhus/sublime-ava) - AVA 테스트 코드 조각
- [atom-ava](https://github.com/sindresorhus/atom-ava) - AVA 테스트 코드 조각
- [vscode-ava](https://github.com/samverschueren/vscode-ava) - AVA 테스트 코드 조각
- [eslint-plugin-ava](https://github.com/sindresorhus/eslint-plugin-ava) - AVA 테스트를 위한 린팅 규칙
- [gulp-ava](https://github.com/sindresorhus/gulp-ava) - gulp와 함께 테스트 실행
- [grunt-ava](https://github.com/sindresorhus/grunt-ava) - grunt와 함께 테스트 실행
- [fly-ava](https://github.com/pine613/fly-ava) - fly와 함께 테스트 실행
- [start-ava](https://github.com/start-runner/ava) - start와 함께 테스트 실행

[더 알아보기...](https://github.com/sindresorhus/awesome-ava#packages)

## 링크

- [AVA 스티커 구입](https://www.stickermule.com/user/1070705604/stickers)
- [Awesome 리스트](https://github.com/sindresorhus/awesome-ava)
- [JavaScript Air 팟캐스트 에피소드](http://jsair.io/ava)

## 팀

[![Sindre Sorhus](https://avatars.githubusercontent.com/u/170270?s=130)](http://sindresorhus.com) | [![Vadim Demedes](https://avatars.githubusercontent.com/u/697676?s=130)](https://github.com/vdemedes) | [![James Talmage](https://avatars.githubusercontent.com/u/4082216?s=130)](https://github.com/jamestalmage) | [![Mark Wubben](https://avatars.githubusercontent.com/u/33538?s=130)](https://novemberborn.net) | [![Juan Soto](https://avatars.githubusercontent.com/u/8217766?s=130)](https://juansoto.me)
---|---|---|---|---|---
[Sindre Sorhus](http://sindresorhus.com) | [Vadim Demedes](https://github.com/vdemedes) | [James Talmage](https://github.com/jamestalmage) | [Mark Wubben](https://novemberborn.net) | [Juan Soto](https://juansoto.me)

### 이전 팀

- [Kevin Mårtensson](https://github.com/kevva)

<div align="center">
	<br>
	<br>
	<br>
	<img src="https://cdn.rawgit.com/sindresorhus/ava/fe1cea1ca3d2c8518c0cc39ec8be592beab90558/media/logo.svg" width="200" alt="AVA">
	<br>
	<br>
</div>
