___
**역자주**

이 문서는 [babel.md](https://github.com/avajs/ava/blob/main/docs/recipes/babel.md)의 한국어 번역입니다. [이곳](https://github.com/avajs/ava/compare/71404c23302d825095659c70cb9a1b08251697ad...main#diff-0730bb7c2e8f9ea2438b52e419dd86c9)에서 AVA의 master 브랜치와 이 문서의 차이를 확인할 수 있습니다. (만약 차이가 없다면 문서가 최신 버전임을 의미합니다)
___

# AVA에서 바벨 설정하기

`@babel/register`, `@babel/core` 패키지를 설치하여 바벨을 활성화 할 수 있습니다.

AVA 설정의 `@babel/register`에 아래 설정을 추가하세요.

**`package.json`:**

```json
{
	"ava": {
		"require": [
			"@babel/register"
		]
	}
}
```

`@babel/register`는 CommonJS와만 호환됩니다.

해당 패키지는 require 호출을 가로채 소스 파일들을 실행 중(on the fly)에 컴파일합니다.

이것은 source, helper, test 파일들을 컴파일할 것입니다.

[`Babel 문서`](https://babeljs.io/docs/en/babel-register)에서 더 많은 정보를 알아보세요.
