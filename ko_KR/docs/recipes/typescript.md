___
**역자주**

이 문서는 [typescript.md](https://github.com/avajs/ava/blob/main/docs/recipes/typescript.md)의 한국어 번역입니다. [이곳](https://github.com/avajs/ava/compare/71404c23302d825095659c70cb9a1b08251697ad...main#diff-0730bb7c2e8f9ea2438b52e419dd86c9)에서 AVA의 master 브랜치와 이 문서의 차이를 확인할 수 있습니다. (만약 차이가 없다면 문서가 최신 버전임을 의미합니다)
___

# TypeScript

AVA는 타입스크립트 definition 파일을 제공합니다. 이를 통해 개발자는 테스트 작성에 TypeScript를 활용할 수 있습니다.

이 가이드에서는 이미 프로젝트에 TypeScript가 설정되어 있다고 가정합니다. 최소 AVA 4.4 이상의 버전을 사용해야 하는 것에 유의하세요.

## 테스트 파일에 AVA 사용하기

### precompile과 함께 사용하기

기본적으로, AVA는 타입스크립트 테스트 파일들을 바로 로드할 수 없습니다.

`tsc` 명령어를 사용해 타입스크립트 파일을 precompile 하는 프로젝트에서 AVA를 사용하기 위해, [`@ava/typescript`] 패키지를 사용할 수 있습니다.

패키지 설치를 위해 [`@ava/typescript`]를 참조하세요.

### `ts-node` 사용하기

트랜스컴파일 없이 바로 테스트 하기 위해 [`ts-node`]를 사용할 수도 있습니다.

이것은 특히 번들러(bundler)를 사용할 때 유용합니다.

아래의 필수 `devDependencies`를 추가해야 함에 유의하세요.

`npm install --save-dev typescript ts-node`

아래의 설치 과정은 당신의 모듈의 `type`에 의존합니다.

1. ["module" 타입의 패키지의 경우](#module-타입의-패키지의-경우)
2. ["module" 타입이 아닌 경우](#module-타입이-아닌-경우)

#### `module` 타입의 패키지의 경우

만약 `package.json`이 `"type": "module"`을 포함한다면, AVA 설정에 아래와 같은 설정이 필요합니다.

`package.json`:

```json
{
  "ava": {
    "extensions": {
      "ts": "module"
    },
    "nodeArguments": [
      "--loader=ts-node/esm"
    ]
  }
}
```

또한 `tsconfig.json`에 아래와 같은 설정이 필요합니다.

```json
{
  "compilerOptions": {
    "module": "ES2020",
    "moduleResolution": "node"
  }
}
```

기본적으로 ES 모듈은 파일 확장자를 지정하도록 요구하며, 타입스크립트 파일은 `.js` 파일들을 출력하므로, `.ts`가 아닌 `.js` 파일들을 import 해야한다는 것을 기억하세요.

만약 이 설정이 마음에 들지 않는다면, Node.js의 _experimental_ 옵션을 사용할 수 있습니다.

테스트가 실행될 때 적용될 AVA 설정의 `nodeArguments` 배열에 해당 옵션을 추가하세요.: [`--experimental-specifier-resolution=node`](https://nodejs.org/api/esm.html#customizing-esm-specifier-resolution-algorithm).

#### `module` 타입이 아닌 경우

만약 `package.json`이 `"type": "module"`을 포함하지 않는 경우, 아래와 같은 AVA 설정이 필요합니다.

`package.json`:

```json
{
  "ava": {
    "extensions": [
      "ts"
    ],
    "require": [
      "ts-node/register"
    ]
  }
}
```

이 설정을 사용했을 때, 타입스크립트 빌드 에러가 난다면 테스트가 실패하게 된다는 것을 알아두세요.

만약 이러한 에러들을 무시하고 테스트하기를 원한다면, `ts-node/register` 대신 `ts-node/register/transpile-only`를 사용하세요.

## 테스트 작성하기

[![StackBlitz 예제 코드 보기](https://developer.stackblitz.com/img/open_in_stackblitz.svg)](https://stackblitz.com/github/avajs/ava/tree/main/examples/typescript-basic?file=source%2Ftest.ts&terminal=test&view=editor)

`test.ts` 파일을 만드세요.

```ts
import test from 'ava';

const fn = () => 'foo';

test('fn() returns foo', t => {
  t.is(fn(), 'foo');
});
```

## [macros](https://github.com/avajs/ava/blob/main/docs/01-writing-tests.md#reusing-test-logic-through-macros) 사용하기

매크로는 여러 부가적인 인자들을 받을 수 있습니다.

AVA는 매크로를 올바르게 사용하는 것을 보장하기 위해, 이런 인자들을 유추할 수 있습니다.

```ts
import test, {ExecutionContext} from 'ava';

const hasLength = (t: ExecutionContext, input: string, expected: number) => {
  t.is(input.length, expected);
};

test('bar has length 3', hasLength, 'bar', 3);
```

그러나, `test.macro()` helper 함수를 통해, 더 나은 타입 추론이 가능합니다.

```ts
import test from 'ava';

const macro = test.macro((t, input: string, expected: number) => {
  t.is(eval(input), expected);
});

test('title', macro, '3 * 3', 9);
```

또는 `title` 함수를 사용하여 아래와 같이 쓸 수 있습니다.

```ts
import test from 'ava';

const macro = test.macro({
  exec(t, input: string, expected: number) {
    t.is(eval(input), expected);
  },
  title(providedTitle = '', input, expected) {
    return `${providedTitle} ${input} = ${expected}`.trim();
  }
});

test(macro, '2 + 2', 4);
test(macro, '2 * 3', 6);
test('providedTitle', macro, '3 * 3', 9);
```

## [`t.context`](https://github.com/avajs/ava/blob/main/docs/01-writing-tests.md#test-context) Typing 하기

[![StackBlitz 예제 코드 보기](https://developer.stackblitz.com/img/open_in_stackblitz.svg)](https://stackblitz.com/github/avajs/ava/tree/main/examples/typescript-context?file=source%2Ftest.ts&terminal=test&view=editor)

기본적으로, `t.context`의 타입은 빈 객체 (`{}`)입니다.

AVA에는 `TestFn<Context>`이라는 인터페이스가 존재(expose)합니다.

해당 인터페이스를 사용해 `t.context`에 custom 타입을 사용할 수 있습니다.

이것은 컴파일 타임에 에러를 잡을 수 있도록 도와줍니다.

```ts
import anyTest, {TestFn} from 'ava';

const test = anyTest as TestFn<{foo: string}>;

test.beforeEach(t => {
  t.context = {foo: 'bar'};
});

test.beforeEach(t => {
  t.context.foo = 123; // error:  Type '123' is not assignable to type 'string'
});

test.serial.failing('very long chains are properly typed', t => {
  t.context.fooo = 'a value'; // error: Property 'fooo' does not exist on type ''
});

test('an actual test', t => {
  t.deepEqual(t.context.foo.map(c => c), ['b', 'a', 'r']); // error: Property 'map' does not exist on type 'string'
});
```

위의 타입 캐스팅에도 불구하고, 따로 할당해주지 않는 한 실행될 때 `t.context`는 빈 객체임에 유의하세요.

## `throws` 단언문 Typing 하기

`t.throws()`와 `t.throwsAsync()` 단언문들은 항상 Error를 리턴하도록 타이핑 되어 있습니다.

이 에러 클래스의 타입을 제네릭을 사용해 커스터마이징 할 수 있습니다.

```ts
import test from 'ava';

class CustomError extends Error {
  parent: Error

  constructor(parent) {
    super(parent.message);
    this.parent = parent;
  }
}

function myFunc() {
  throw new CustomError(new TypeError('🙈'));
};

test('throws', t => {
  const err = t.throws<CustomError>(myFunc);
  t.is(err.parent.name, 'TypeError');
});

test('throwsAsync', async t => {
  const err = await t.throwsAsync<CustomError>(async () => myFunc());
  t.is(err.parent.name, 'TypeError');
});
```

이런 typing에도 불구하고, 해당 단언문이 실패하게 되면 `undefined`를 리턴한다는 것에 유의하세요.

`Error | undefined`를 리턴하는 단언문을 typing 하는 것은 실용적인 선택이 아닙니다.

### module path mapping 사용하기

[`ts-node`는 module path mapping를 지원하지 않습니다](https://github.com/TypeStrong/ts-node/issues/138).

그러나 [`tsconfig-paths`](https://github.com/dividab/tsconfig-paths#readme)를 사용하면 module path mapping이 가능합니다.

설정 후, AVA 설정의 `require` 항목에 `tsconfig-paths/register`를 추가하세요.

`package.json`:

```json
{
  "ava": {
    "extensions": [
      "ts"
    ],
    "require": [
      "ts-node/register",
      "tsconfig-paths/register"
    ]
  }
}
```

그럼, 아래와 같이 module aliases를 설정할 수 있게 됩니다.

`tsconfig.json`:
```json
{
  "baseUrl": ".",
  "paths": {
    "@helpers/*": ["helpers/*"]
  }
}
```

아래와 같이 테스트 파일에서 module aliases를 사용할 수 있습니다.

```ts
import myHelper from '@helpers/myHelper';

// 파일의 나머지
```

[`@ava/typescript`]: https://github.com/avajs/typescript
[`ts-node`]: https://www.npmjs.com/package/ts-node
