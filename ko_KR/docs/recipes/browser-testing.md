___
**역자주**

이 문서는 [browser-testing.md](https://github.com/avajs/ava/blob/main/docs/recipes/browser-testing.md)의 한국어 번역입니다. [이곳](https://github.com/avajs/ava/compare/71404c23302d825095659c70cb9a1b08251697ad...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9)에서 AVA의 master 브랜치와 이 문서의 차이를 확인할 수 있습니다. (만약 차이가 없다면 문서가 최신 버전임을 의미합니다)
___
# 브라우저 테스팅을 위해 AVA 설정하기

전체 번역 문서들: [English](https://github.com/avajs/ava/raw/main/docs/recipes/browser-testing.md), [Italiano](https://github.com/avajs/ava-docs/blob/master/it_IT/recipes/browser-testing.md), [Русский](https://github.com/avajs/ava-docs/blob/master/ru_RU/docs/recipes/browser-testing.md), [简体中文](https://github.com/avajs/ava-docs/blob/master/zh_CN/docs/recipes/browser-testing.md), [한국어](https://github.com/avajs/ava-docs/blob/master/ko_KR/docs/recipes/browser-testing.md)

AVA는 [아직](https://github.com/avajs/ava/issues/24) 브라우저에서 테스트를 실행하는 방법을 지원하지 않고 있습니다. 몇 가지 라이브러리는 브라우저 특정 전역 변수를 요구합니다. (`window`, `document`, `navigator`, 등) 예를 들면 React가 있습니다. 아마 최소한 ReactDOM.render를 사용하고 ReactTestUtils를 통해 이벤트를 시뮬레이션하고 싶을 것입니다.

이 레시피는 모킹된 브라우저 환경을 필요로하는 모든 라이브러리에서 작동합니다.

## jsdom 설치

[jsdom](https://github.com/tmpvar/jsdom)을 설치합니다.

> WHATWG DOM과 HTML 표준의 JavaScript 구현입니다. node.js와 함께 사용할 수 있습니다.

```
$ npm install --save-dev jsdom
```

## jsdom 설정하기

`test/helpers` 폴더에 헬퍼 파일을 만듭니다. 이 폴더에 넣으면 AVA가 테스트로 오인하지 않도록 해줍니다.

`test/helpers/setup-browser-env.js`:

```js
global.document = require('jsdom').jsdom('<body></body>');
global.window = document.defaultView;
global.navigator = window.navigator;
```

## 테스트가 jsdom을 사용하도록 설정하기

AVA가 각각의 헬퍼 파일을 테스트 파일로 `require` 하도록 설정합니다.

`package.json`:

```json
{
  "ava": {
    "require": [
      "./test/helpers/setup-browser-env.js"
    ]
  }
}
```

## 즐기세요!

테스트를 작성하고 모킹된 window 객체를 즐깁니다.

`test/my.react.test.js`:

```js
import test from 'ava';
import React from 'react';
import {render} from 'react-dom';
import {Simulate} from 'react-addons-test-utils';
import sinon from 'sinon';
import CustomInput from './components/custom-input.jsx';

test('Input appelle onBlur', t => {
	const onUserBlur = sinon.spy();
	const input = render(
		React.createElement(CustomInput, onUserBlur),
		div
	);

	Simulate.blur(input);

	t.true(onUserBlur.calledOnce);
});
```
