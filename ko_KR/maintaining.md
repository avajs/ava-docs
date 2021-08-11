___
**역자주**

이 문서는 [maintaining.md](https://github.com/avajs/ava/blob/main/maintaining.md)의 한국어 번역입니다. [이곳](https://github.com/avajs/ava/compare/71404c23302d825095659c70cb9a1b08251697ad...master#diff-0730bb7c2e8f9ea2438b52e419dd86c9)에서 AVA의 master 브랜치와 이 문서의 차이를 확인할 수 있습니다. (만약 차이가 없다면 문서가 최신 버전임을 의미합니다)
___
# 유지하기 [![Dependency Status](https://david-dm.org/avajs/ava.svg)](https://david-dm.org/avajs/ava) [![devDependency Status](https://david-dm.org/avajs/ava/dev-status.svg)](https://david-dm.org/avajs/ava#info=devDependencies)

## 행동강령

**모두를 친절하게 대하세요.**
자세한 내용은 [행동강령](code-of-conduct.md) 문서를 참고하고 기여자 규약에 동의 해주세요.

## 테스트하기

 - `npm test`: 코드를 검사하고 커버리지와 함께 모든 테스트 코드를 실행합니다.
 - `npm run test-win`: Windows에서 테스트를 실행합니다.
 - `npm run coverage`: 마지막 테스트의 커버리지 리포트를 생성합니다. (브라우저 윈도우가 열립니다)
 - `tap test/fork.js --bail`: 특정한 테스트 파일을 실행하고 첫 번째 실패를 보증합니다. (버그를 찾을 때 유용합니다)

## 릴리즈 과정

- 종송성을 업데이트합니다.
- [Travis CI](https://travis-ci.org/avajs/ava)와 [AppVeyor](https://ci.appveyor.com/project/avajs/ava/branch/master)의 상태가 정상인지 확인합니다.
- [semver](http://semver.org)에 따라 [`np`](https://github.com/sindresorhus/np)을 사용하여 새로운 버전을 게시합니다.
- 이전 릴리즈 노트의 형식에 따라 [릴리즈 노트](https://github.com/avajs/ava/releases/new)를 작성합니다.

## Pull Request

- 새로운 기능은 반드시 테스트를 제공해야 하고 문서화해야 합니다.
- [기여자 가이드라인](contributing.md)을 따랐는지 확인합니1다.
- 최소한 한 명 이상의 팀원이 pull request가 합쳐지기 전에 `LGTM`을 해주어야 합니다.
- 합치기 전에 커밋들을 squash 합니다. *[예시](https://github.com/avajs/ava/commit/0675d3444da6958b54c7e5eada91034e516bc97c)*

## 이슈 라벨

이슈를 분류할 때 라벨을 추가합니다:

* `babel`: Babel 하부 구조에서 발생항 문제는 이 라벨을 사용합니다.
* `blocked`: 이슈가 막혔을 때 이 라벨을 사용합니다. 코멘트와 또는 이슈를 수정하여 무엇이 이슈를 막고있는지 설명을 남겨주세요.
* `bug`: AVA에 대한 버그는 이 라벨을 사용합니다.
* `DO NOT MERGE`: 합쳐지면 안 되는, 확인이 필요한 pull request에 이 라벨을 사용합니다.
* `docs`: 문서의 개선을 추적할 때 이 라벨을 사용합니다.
* `enhancement`: 기능 요청에는 이 라벨을 사용합니다.
* `good for beginner`: 입문자에게 유용한 이슈에는 이 라벨을 사용합니다.
* `help wanted`: 코어 팀을 떠나서 외부의 도움을 받아야할 때 이 라벨을 사용합니다.
* `performance`: 성능에 관련된 이슈에는 이 라벨을 사용합니다.
* `question`: 토론 딘계의 이슈에는 이 라벨을 사용합니다.

다음 라벨들은 우선순위를 표시합니다:

* `priority`: 가능한 한 빠르게 해결해야 하는 이슈.
* `low priority`: 천천히 진전적으로 해결할 이슈.
* `future`: 조만간에 예정되어있지 않은 이슈. 현재로써는 PR을 받아들일 것 같지 않은 긴 기간의 유예기간을 두어야 하는 제안 사항.

`assigned` 라벨은 누군가가 이슈에 작업하고 있을 때 사용하며 중복된 작업을 피하기 위해 사용할 수 있습니다.

## 프로파일하기

먼저 [`iron-node`](https://github.com/s-a/iron-node) 그리고 / 또는 [`devtool`](https://github.com/Jam3/devtool)를 전역에 설치해야 합니다:

```
$ npm install --global iron-node devtool
```

AVA를 사용하는 프로젝트의 루트에서 다음을 실행합니다:

```
$ iron-node node_modules/ava/profile.js <test-file>
```

또는:

```
$ devtool node_modules/ava/profile.js <test-file>
```

개발자 도구 창이 로드되면, Memory나 CPU 프로파일링을 활성화하고 <kbd>Cmd</kbd> <kbd>R</kbd>를 입력하면 테스트를 다시 시작합니다.

테스트가 완료되면, 레코딩을 중지하고 프로파일러 결과를 검사합니다. `Profiles` 탭의 드롭다운 메뉴에서 `Chart`를 선택하면 flamegraph를 볼 수 있습니다. (다른 뷰는 `Tree (top down)`와 `Heavy (bottom up)`를 포함하고 있습니다)

필요하다면 개발자 도구의 Settings 페이지에서 여러 가지 기능을 확인하거나 Profiling 섹션에서 한 개 또는 여러 옵션을 활성화할 수도 있습니다.

##### 도움될만한 정보들

 - [An introduction to Node.js debugging with `devtool`](http://mattdesl.svbtle.com/debugging-nodejs-in-chrome-devtools).
 - [A video introduction to Chrome DevTools CPU and Memory profiling](https://www.youtube.com/watch?v=KKwmdTByxLk).

## 밴치마킹하기

먼저 branch/commit에 대한 벤치마크 데이터를 수집합니다:

```
$ node bench/run
```

branches/commits에서 두/세 가지 데이터를 수집한 후:

```
$ node bench/compare
```

*작업중인 트리와 마지막 커밋에서 예시 벤치마크 데이터를 얻을 수 있습니다.*

![](https://cloud.githubusercontent.com/assets/4082216/12700805/bf18f730-c7bf-11e5-8a4f-fec0993c053f.png)

이제 스위트의 일부를 실행할 수 있습니다:

```
$ node bench/run.js concurrent/sync.js serial/sync.js -- concurrent/sync.js -- serial/sync.js
```

참고로 `--` 분리자에 대해 설명을 하자면, 위 명령은 다음 벤치마크 명령과 같은 작업을 수행합니다:

```
$ ava concurrent/sync.js serial/sync.js
$ ava concurrent/sync.js
$ ava serial/sync.js
```

또한 스위트가 반드시 실패할때를 벤치마크 하려면, 다음 그룹에 `--should-fail`를 추가해야 합니다:

```
$ node bench/run.js concurrent/sync.js -- --should-fail other/failures.js
```

위는 두 명령을 벤치마크하지만, 두 번째는 실패합니다.

## 새로운 핵심 멤버 등록하기

- `readme.md`와 `package.json`에 사용자를 추가합니다.
- 모든 AVA와 관련된 저장소와 npm 패키지에 사용자를 콜라보레이터로 추가합니다.
- 트위터 계정 로그인 정보를 공유하고 관련된 것들의 트윗/리트윗을 장려합니다.
