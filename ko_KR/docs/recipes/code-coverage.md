___
**역자주**

이 문서는 [code-coverage.md](https://github.com/avajs/ava/blob/main/docs/recipes/code-coverage.md)의 한국어 번역입니다. [이곳](https://github.com/avajs/ava/compare/71404c23302d825095659c70cb9a1b08251697ad...main#diff-0730bb7c2e8f9ea2438b52e419dd86c9)에서 AVA의 master 브랜치와 이 문서의 차이를 확인할 수 있습니다. (만약 차이가 없다면 문서가 최신 버전임을 의미합니다)
___

# 코드 커버리지

[`c8`]을 사용해 테스트의 코드 커버리지를 계산하세요.

아래 명령어로 [`c8`]을 설치하세요.

```
$ npm install --save-dev c8
```

가장 단순하게는, [`c8`]을 통해 AVA를 실행할 수 있습니다. 당신의 `package.json` 파일에 아래처럼 `c8 ava`을 추가하세요.

```json
{
	"scripts": {
		"test": "c8 ava"
	}
}
```

소스 컨트롤에서 `coverage` 디렉토리를 제거하고 싶다면, git을 사용 중인 경우 `.gitignore`에 아래 라인을 추가하세요.

```
coverage
```

[`c8`]: https://github.com/bcoe/c8
