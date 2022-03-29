___
**역자주**

이 문서는 [06-configuration.md](https://github.com/avajs/ava/blob/main/docs/06-configuration.md)의 한국어 번역입니다. [이곳](https://github.com/avajs/ava/compare/71404c23302d825095659c70cb9a1b08251697ad...main#diff-0730bb7c2e8f9ea2438b52e419dd86c9)에서 AVA의 master 브랜치와 이 문서의 차이를 확인할 수 있습니다. (만약 차이가 없다면 문서가 최신 버전임을 의미합니다)
___

# 설정

모든 [CLI 옵션](CLI)들은 `package.json`이나 `ava.config.*`의 `ava` 섹션을 통해 설정할 수 있습니다.

이 설정들은 `ava` 명령어의 기본적인 동작을 수정해, 명령줄의 같은 옵션을 여러 번 반복해 타이핑 하지 않을 수 있게 만들어 줍니다.

특정 파일들을 무시하기 위해서, `!`를 패턴 앞에 붙여 사용하세요.

**`package.json`:**

```json
{
	"ava": {
		"files": [
			"test/**/*",
			"!test/exclude-files-in-this-directory",
			"!**/exclude-files-with-this-name.*"
		],
		"match": [
			"*oo",
			"!foo"
		],
		"concurrency": 5,
		"failFast": true,
		"failWithoutAssertions": false,
		"environmentVariables": {
			"MY_ENVIRONMENT_VARIABLE": "some value"
		},
		"verbose": true,
		"require": [
			"./my-helper-module.js"
		],
		"nodeArguments": [
			"--trace-deprecation",
			"--napi-modules"
		]
	}
}
```

CLI에 전달된 인자들은 언제나 `package.json`에 설정된 설정들보다 우선시 됩니다.

## 옵션

- `files`: 테스트 파일을 선택하는 패턴(glob) 배열입니다. `_` 접두사가 있는 파일은 무시됩니다. 패턴이 다른 파일과 일치하더라도 기본적으로 확장자가 `cjs`, `mjs` 및 `js`인 파일만 선택합니다. 다른 파일 확장명을 허용하려면 `extensions`를 지정하십시오.

- `ignoredByWatcher`: 변경하더라도 Watcher에 의해 무시되는 파일에 일치하는 패턴 배열입니다. [watch mode 상세 레시피](https://github.com/avajs/ava/blob/main/docs/recipes/watch-mode.md)를 참조하세요.

- `match`: 일반적으로 `package.json` 설정에서 유용하게 사용되진 않지만, [CLI에서 `--match` 명시하기](./05-command-line.md#title에-매칭되는-테스트들-실행하기)의 내용과 동일합니다.

- `cache`: 기본적으로 `true`로, 컴파일된 파일들을 `node_modules/.cache/ava` 밑에 캐싱합니다. `false`로 지정하는 경우, 컴파일된 파일들은 임시 디렉토리에 대신 캐싱됩니다.

- `concurrency`: 동시에 실행되는 최대 테스트 파일 수 (기본값: CPU 코어 수).

- `workerThreads`: 워커 스레드를 사용하여 테스트를 실행합니다 (기본적으로 활성화됨). `false`인 경우 자식 프로세스에서 테스트를 실행합니다.

- `failFast`: 테스트가 실패하면 추가 테스트들의 실행을 중지 합니다.

- `failWithoutAssertions`: `false`인 경우, [단언문](./03-assertions.md)을 실행하지 않아도 테스트에 실패하지 않습니다.

- `environmentVariables`: 테스트에 사용할 환경 변수를 지정합니다. 여기서 정의된 환경 변수는 `process.env`의 환경 변수를 재정의합니다.

- `tap`: `true`인 경우, [TAP reporter](./05-command-line.md#tap-reporter)를 활성화 합니다.

- `verbose`: `true`인 경우, verbose 출력을 활성화 합니다. (다만 non-verbose 출력은 현재 지원되지 않습니다.)

- `snapshotDir`: 스냅샷 파일을 저장할 고정 위치를 지정합니다. 스냅샷이 잘못된 위치에 저장된다면 이 옵션을 사용하세요.

- `extensions`: 테스트 파일의 확장자. 이 값을 설정하면 기본 확장자 [`cjs`, `mjs`, `js`] 값이 재정의되므로 이러한 확장명을 목록에 포함해야 합니다. [실험적으로 파일 로드 방법을 설정할 수 있습니다.](#configuring-module-formats)

- `require`: 테스트를 실행하기 전에 필요한 추가 모듈. [워커 프로세스](./01-writing-tests.md#테스트-고립-isolation)에 필요한 추가 모듈을 명시하세요.

- `timeout`: AVA의 시간 초과는 다른 테스트 프레임워크와 다르게 작동합니다. AVA는 각 테스트 후 타이머를 재설정하여 지정된 제한 시간 내에 새 테스트 결과가 수신되지 않으면 테스트를 강제로 종료합니다. 정지된 테스트를 처리하는 데 사용할 수 있습니다. 더 상세한 설명은 [timeout documentation](./07-test-timeouts.md)를 참조하세요.

- `nodeArguments`: 워커 프로세스를 시작하는 데 사용되는 Node.js 인수를 구성합니다.

- `sortTestFiles`: 테스트 파일 정렬을 위한 comparator 함수. `ava.config.*` 파일에서만 사용 가능합니다. 사용 예제는 [이곳](https://github.com/avajs/ava/blob/main/docs/recipes/splitting-tests-ci.md)을 참고하세요.

- `utilizeParallelBuilds`: `false`로 설정된 경우, 병렬 빌드(parallel build) 기능을 비활성화 합니다.

CLI에서 파일들을 제공하는 것이 `files` 옵션보다 우선됨에 유의하세요.

AVA가 타입스크립트 파일을 실행할 수 있게 하기 위해, `typescript` 옵션을 제공하세요. (그리고 [`@ava/typescript`](https://github.com/avajs/typescript)를 추가적인 디펜던시(dependency)로 설치하세요.)

## `ava.config.*` 사용하기

`package.json` 파일에 설정을 명시하는 것 대신, `ava.config.js`, `ava.config.cjs`, `ava.config.mjs` 파일들을 사용할 수 있습니다.

이러한 파일들을 사용하기 위해서 아래처럼 하세요:

1. `package.json`는 `ava` 프로퍼티를 포함해선 안 됩니다. (포함한다면 빈 객체로 만들어야 합니다.)

2. 어떤 디렉토리에도 `ava.config.*` 파일이 2개 이상 존재하면 안 됩니다, 즉, `ava.config.js` *과* `ava.config.cjs` 파일들을 섞어 쓰지 마세요.

AVA는 당신의 파일 시스템에서 `ava.config.*` 파일들을 검색합니다.

우선, AVA를 실행할 때, 가장 가까운 `package.json` 파일을 찾습니다.

이 디렉터리에서 시작하여 파일 시스템 루트에 도달하거나 `.git` 파일, 디렉터리에 도달할 때까지 부모 디렉터리를 재귀적으로 검사합니다.

첫 번째로 찾은 `ava.config.*` 파일이 선택됩니다.

이것은 단일 설정 파일을 모노 레포(monorepo) 셋업에서 사용할 수 있게 해 줍니다.

### `ava.config.js`

AVA는 Node.js의 행동을 따릅니다. 따라서, `"type": "module"`를 명시하면, ESM을 사용해야만 합니다. 그렇지 않으면 CommonJS를 사용해야 합니다.

해당 파일의 default export는 플레인 객체(plain object) 이거나, 플레인 객체를 리턴하는 팩토리 함수가 될 수 있습니다.

플레인 객체를 export 하거나, 플레인 객체에 대한 프라미스를 리턴할 수 있습니다:

```js
export default {
	require: ['./_my-test-helper']
};
```

```js
export default function factory() {
	return {
		require: ['./_my-test-helper']
	};
};
```

팩토리(factory) 함수는 리턴 받은 설정을 변경하는데 사용할 수 있는 `projectDir` 프로퍼티를 포함하는 개체와 함께 호출됩니다.:

```js
export default ({projectDir}) => {
	if (projectDir === '/Users/username/projects/my-project') {
		return {
			// 설정 A
		};
	}

	return {
		// 설정 B
	};
};
```

### `ava.config.cjs`

`ava.config.cjs` 파일에서, `module.exports`을 할당해야 합니다. ["Module scope"](https://nodejs.org/docs/latest-v12.x/api/modules.html#modules_the_module_scope)를 사용할 수 있습니다. `require()` 디펜던시를 사용할 수 있습니다.

해당 파일의 `module.exports`는 플레인 객체(plain object) 이거나, 플레인 객체를 리턴하는 팩토리 함수가 될 수 있습니다.

플레인 객체를 export 하거나, 플레인 객체에 대한 프라미스를 리턴할 수 있습니다:

```js
module.exports = {
	require: ['./_my-test-helper']
};
```

```js
module.exports = () => {
	return {
		require: ['./_my-test-helper']
	};
};
```

팩토리(factory) 함수는 리턴 받은 설정을 변경하는데 사용할 수 있는 `projectDir` 프로퍼티를 포함하는 개체와 함께 호출됩니다.:

```js
module.exports = ({projectDir}) => {
	if (projectDir === '/Users/username/projects/my-project') {
		return {
			// 설정 A
		};
	}

	return {
		// 설정 B
	};
};
```

### `ava.config.mjs`

해당 파일의 default export는 플레인 객체(plain object) 이거나, 플레인 객체를 리턴하는 팩토리 함수가 될 수 있습니다.

플레인 객체를 export 하거나, 플레인 객체에 대한 프라미스를 리턴할 수 있습니다:

```js
export default {
	require: ['./_my-test-helper']
};
```

```js
export default function factory() {
	return {
		require: ['./_my-test-helper']
	};
};
```

팩토리(factory) 함수는 리턴 받은 설정을 변경하는데 사용할 수 있는 `projectDir` 프로퍼티를 포함하는 개체와 함께 호출됩니다.:

```js
export default ({projectDir}) => {
	if (projectDir === '/Users/username/projects/my-project') {
		return {
			// 설정 A
		};
	}

	return {
		// 설정 B
	};
};
```

## 대안적인 (Alternative) 설정 파일 구성

[CLI]는 `--config` 플래그를 사용해 당신이 특정 설정 파일을 명시할 수 있도록 해 줍니다.

이 파일은 `.js`, `.cjs` 또는 `.mjs` 확장자를 가져야 하며 `ava.config.js`, `ava.config.cjs` 또는 `ava.config.mjs` 파일처럼 처리됩니다.

`--config` 플래그가 셋팅 되면, 제공된 파일이 `package.json`, `ava.config.js`, `ava.config.cjs`, `ava.config.mjs`의 모든 설정을 덮어 씁니다. 설정은 병합 되지 않습니다.

특정 테스트를 실행할 때 설정을 커스터마이징 하기 위해 이 기능을 사용할 수 있습니다.

예를 들어, 통합 테스트와 별도로 단위 테스트를 실행하기를 바랄 수 있습니다.:

`ava.config.cjs`:

```js
module.exports = {
	files: ['unit-tests/**/*']
};
```

`integration-tests.config.cjs`:

```js
const baseConfig = require('./ava.config.cjs');

module.exports = {
	...baseConfig,
	files: ['integration-tests/**/*']
};
```

이제 당신은 `npx ava`를 통해 단위 테스트들을 실행하고, `npx ava --config integration-tests.config.cjs`를 통해 통합 테스트를 사용할 수 있습니다.

## 객체 출력 깊이

기본적으로, AVA는 중첩된 개체를 `3` 깊이로 출력합니다.

그러나 깊이 중첩된 개체로 디버깅 테스트를 수행할 때 더 자세히 출력하는 것이 유용할 수 있습니다.

이것은 테스트 실행 전 [`util.inspect.defaultOptions.depth`](https://nodejs.org/api/util.html#util_util_inspect_defaultoptions)를 원하는 깊이로 설정하는 것으로 이뤄질 수 있습니다.:

```js
const util = require('util');

const test = require('ava');

util.inspect.defaultOptions.depth = 5;  // AVA가 출력하는 객체의 깊이를 증가시킵니다.

test('My test', t => {
	t.deepEqual(someDeeplyNestedObject, theExpectedValue);
});
```

AVA의 최소 깊이는 `3`입니다.

## 실험적인 기능들

때때로, AVA는 실험적인 기능들을 도입할 것입니다.

이것들은 새 메이저 버전이 있을 때 뿐만 아니라 언제든지 변경되거나 제거될 수 있습니다.

`nonSemVerExperiments` 설정을 활성화 함으로써 이러한 기능들을 기본적으로 활성화(opt in) 할 수 있습니다.

`ava.config.js`:
```js
export default {
	nonSemVerExperiments: {
		feature: true
	}
};
```

## 모듈 형식 설정하기

Node.js는 [실험적인 로더](https://nodejs.org/docs/latest/api/esm.html#esm_experimental_loaders)를 사용할 때, 비표준 확장만 ES 모듈로 로드할 수 있습니다.

이를 사용하려면 AVA가 테스트 파일을 `import()` 하도록 설정해야 할 것 입니다.

배열 형태처럼 `js`, `cjs` 및 `mjs` 확장자를 명시적으로 나열해야 합니다.

이 값들은 `true` 값을 **반드시** 가져야만 합니다; 다른 확장들은 `'commonjs'` 나 `'module'`을 사용해 설정될 수 있습니다.

`ava.config.js`:
```js
export default {
	extensions: {
		js: true,
		ts: 'module'
	}
};
```

## Node.js 인자

`nodeArguments` 구성은 워커 프로세스를 시작할 때 추가 인수를 전달하는 데 사용될 수 있습니다.

이것들은 AVA를 시작할 때 CLI에서 전달된 `--node-arguments`와 `node` 바이너리에 전달된 모든 인수와 결합됩니다.

[CLI]: ./05-command-line.md
