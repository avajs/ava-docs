___
**역자주**

이 문서는 [05-command-line.md](https://github.com/avajs/ava/blob/main/docs/05-command-line.md)의 한국어 번역입니다. [이곳](https://github.com/avajs/ava/compare/71404c23302d825095659c70cb9a1b08251697ad...main#diff-0730bb7c2e8f9ea2438b52e419dd86c9)에서 AVA의 master 브랜치와 이 문서의 차이를 확인할 수 있습니다. (만약 차이가 없다면 문서가 최신 버전임을 의미합니다)
___

# CLI

```console
ava [<pattern>...]
ava debug [<pattern>...]
ava reset-cache

Commands:
  ava [<pattern>...]        Run tests                                  [default]
  ava debug [<pattern>...]  Activate Node.js inspector and run a single test
                            file
  ava reset-cache           Delete any temporary files and state kept by AVA,
                            then exit

Positionals:
  pattern  Select which test files to run. Leave empty if you want AVA to run
           all test files as per your configuration. Accepts glob patterns,
           directories that (recursively) contain test files, and file paths
           optionally suffixed with a colon and comma-separated numbers and/or
           ranges identifying the 1-based line(s) of specific tests to run
                                                                        [string]

Options:
      --version            Show version number                         [boolean]
      --color              Force color output                          [boolean]
      --config             Specific JavaScript file for AVA to read its config
                           from, instead of using package.json or ava.config.*
                           files
      --help               Show help                                   [boolean]
  -c, --concurrency        Max number of test files running at the same time
                           (default: CPU cores)                         [number]
      --fail-fast          Stop after first test failure               [boolean]
  -m, --match              Only run tests with matching title (can be repeated)
                                                                        [string]
      --no-worker-threads  Don't use worker threads                    [boolean]
      --node-arguments     Additional Node.js arguments for launching worker
                           processes (specify as a single string)       [string]
  -s, --serial             Run tests serially                          [boolean]
  -t, --tap                Generate TAP output                         [boolean]
  -T, --timeout            Set global timeout (milliseconds or human-readable,
                           e.g. 10s, 2m)                                [string]
  -u, --update-snapshots   Update snapshots                            [boolean]
  -v, --verbose            Enable verbose output (default)             [boolean]
  -w, --watch              Re-run tests when files change              [boolean]

Examples:
  ava
  ava test.js
  ava test.js:4,7-9
```

AVA는 다음의 패턴들로 테스트 파일들을 검색합니다.:

* `test.js`
* `src/test.js`
* `source/test.js`
* `**/test-*.js`
* `**/*.spec.js`
* `**/*.test.js`
* `**/test/**/*.js`
* `**/tests/**/*.js`
* `**/__tests__/**/*.js`

`node_modules` 내부의 파일들은 *언제나* 무시됩니다.

`_`로 시작하는 파일이나 `_`로 시작하는 디렉터리 내부 파일도 마찬가지로 무시됩니다.

추가적으로, 다른 패턴들을 명시하지 않는 한 아래 패턴에 매칭되는 파일들은 기본적으로 무시됩니다.

* `**/__tests__/**/__helper__/**/*`
* `**/__tests__/**/__helpers__/**/*`
* `**/__tests__/**/__fixture__/**/*`
* `**/__tests__/**/__fixtures__/**/*`
* `**/test/**/helper/**/*`
* `**/test/**/helpers/**/*`
* `**/test/**/fixture/**/*`
* `**/test/**/fixtures/**/*`
* `**/tests/**/helper/**/*`
* `**/tests/**/helpers/**/*`
* `**/tests/**/fixture/**/*`
* `**/tests/**/fixtures/**/*`

`npm test`를 사용할 때, `npm test test2.js`과 같이 직접 위치 인자 (positional arguments) 를 전달할 수 있습니다, 그러나 플래그는 `npm test -- --verbose`과 같이 전달되어야 합니다.

## title에 매칭되는 테스트들 실행하기

[![StackBlitz 예제 코드 보기](https://developer.stackblitz.com/img/open_in_stackblitz.svg)](https://stackblitz.com/github/avajs/ava/tree/main/examples/matching-titles?file=test.js&terminal=test&view=editor)

`--match` 플래그로 title이 일치하는 테스트들만 실행 할 수 있습니다.

이것은 간단한 와일드 카드(wildcard) 패턴과 함께 사용될 수 있습니다.

패턴은 대소문자를 구분하지 않습니다. (case insensitive)

([`matcher`](https://github.com/sindresorhus/matcher)를 참조하세요.)

`foo`로 끝나는 title들에 매칭됩니다:

```console
npx ava --match='*foo'
```

`foo`로 시작하는 title들에 매칭됩니다:

```console
npx ava --match='foo*'
```

`foo`를 포함하는 title들에 매칭됩니다:

```console
npx ava --match='*foo*'
```

`foo`에 정확하게 매칭되는 테스트들만 매칭됩니다. (대소문자 구분 안 함):

```console
npx ava --match='foo'
```

`foo`를 포함하지 않는 title들에 매칭됩니다:

```console
npx ava --match='!*foo*'
```

`foo`를 포함하고, `bar`로 끝나는 테스트들에 매칭됩니다:

```console
npx ava --match='foo*bar'
```

`foo`를 포함하거나 `bar`로 끝나는 테스트들에 매칭됩니다:

```console
npx ava --match='foo*' --match='*bar'
```

패턴 매치가 `.only` 제어자보다 우선된다는 것을 유의하세요.

오직 명시적인 title을 가진 테스트들만 매칭됩니다.

title을 생략하거나, 구현 함수에서 전달된 타이틀을 가진 테스트들은 매칭되지 않을 것입니다.

AVA를 `*oo*` 패턴으로, 아래 테스트들에 실행했을 때 일어나는 일은 다음과 같습니다.:

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

// title이 없으므로 실행되지 않음
test(function (t) {
	t.fail();
});

// title이 명시적으로 주어지지 않았으므로 실행되지 않음
test(function foo(t) {
	t.fail();
});
```

## 특정 줄 번호의 테스트들 실행하기

[![StackBlitz 예제 코드 보기](https://developer.stackblitz.com/img/open_in_stackblitz.svg)](https://stackblitz.com/github/avajs/ava/tree/main/examples/specific-line-numbers?file=test.js&terminal=test&view=editor)

AVA를 통해 특정 줄 번호의 테스트들만 실행할 수 있습니다.

단일 라인, 여러 라인 또는 둘 모두를 대상으로 할 수 있습니다.

테스트의 어떤 라인도 선택할 수 있습니다.

`[X|Y-Z]` 형태의 포맷으로 전달 할 수 있습니다. (`X`, `Y`, `Z`는 1부터 마지막 줄 번호 사이의 정수)

이 기능은 커맨드 라인으로부터만 이용 가능합니다.

### 단일 테스트 실행하기

어떤 파일 내부의 특정 테스트만 실행하기 위해서, AVA에 주어질 경로나 패턴에 테스트의 줄 번호를 넘겨주세요.

아래의 테스트 파일이 주어졌을 때:

`test.js`

```js
1: test('unicorn', t => {
2:   t.pass();
3: });
4:
5: test('rainbow', t => {
6:  t.fail();
7: });
```

`npx ava test.js:2`를 실행하면 `unicorn` 테스트가 실행됩니다.

사실 1과 3 사이의 어떤 줄 번호를 넘겨줘도 `unicorn` 테스트가 실행됩니다.

### 여러 테스트들 실행하기

여러 테스트들을 실행할 때, 1개씩, 또는 특정 라인 범위를 대상으로 선택하세요.

파일마다 다른 줄 번호를 지정해, 각 파일에 대해 다른 줄 번호들을 적용해 실행할 수 있습니다.

만약 같은 파일이 여러 번 지정되면, 줄 번호들은 병합되어 한 번만 실행됩니다.

### 예제

하나의 줄 번호:

```console
npx ava test.js:2,9
```

범위:

```console
npx ava test.js:4-7
```

하나의 줄 번호와, 범위를 섞여 사용하는 경우:

```console
npx ava test.js:4,9-12
```

여러 파일을 사용하는 경우:

```console
npx ava test.js:3 test2.js:4,7-9
```

같은 파일을 줄 번호를 사용하는 경우와, 사용하지 않는 경우로 두 번 실행하면, 줄 번호를 사용하는 쪽이 먼저 실행됩니다.

## AVA의 캐시 리셋하기

AVA는 일시적인 상태를 캐시에 유지합니다. 이 상태를 아래 명령줄을 실행해 제거할 수 있습니다.

```console
npx ava reset-cache
```

위 명령어는 `node_modules/.cache/ava`의 모든 파일들을 제거합니다.

## Reporters

AVA는 읽기 쉬운(human readable) reporter를 기본적으로 제공합니다:

<img src="https://raw.githubusercontent.com/avajs/ava/main/media/verbose-reporter.png" width="294">

### TAP reporter

[![StackBlitz 예제 코드 보기](https://developer.stackblitz.com/img/open_in_stackblitz.svg)](https://stackblitz.com/github/avajs/ava/tree/main/examples/tap-reporter?file=test.js&terminal=test&view=editor)

AVA는 TAP format을 지원하기 때문에, [어떤 종류의 TAP reporter](https://github.com/sindresorhus/awesome-tap#reporters)와도 호환됩니다.

TAP 출력을 활성화하기 위해 `--tap` 플래그를 사용하세요.

```console
$ npx ava --tap | npx tap-nyan
```

<img src="https://raw.githubusercontent.com/avajs/ava/main/media/tap-reporter.png" width="420">

TAP reporter는 [감시 모드 (Watch mode)](./recipes/watch-mode.md)에서 이용할 수 없다는 것에 유의해 주세요.

## Node.js 인자

`--node-arguments` 인자는 워커 프로세스를 실행할 때 전달될 추가 인자들을 명시하는 데 사용될 수 있습니다.

해당 인자들은 `nodeArguments`와 함께 결합되며, AVA를 실행할 때 `node` 바이너리에 전달됩니다.

**오직 믿을 수 있는 값들만 전달하세요.**

하나의 문자열로 인자들을 명시하세요:

```console
npx ava --node-arguments="--throw-deprecation --zero-fill-buffers"
```

**오직 믿을 수 있는 값들만 전달하세요.**
