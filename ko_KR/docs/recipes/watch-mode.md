___
**역자주**

이 문서는 [watch-mode.md](https://github.com/avajs/ava/blob/main/docs/recipes/watch-mode.md)의 한국어 번역입니다. [이곳](https://github.com/avajs/ava/compare/71404c23302d825095659c70cb9a1b08251697ad...main#diff-0730bb7c2e8f9ea2438b52e419dd86c9)에서 AVA의 master 브랜치와 이 문서의 차이를 확인할 수 있습니다. (만약 차이가 없다면 문서가 최신 버전임을 의미합니다)
___

# 감시 모드 (Watch mode)

AVA는 지능형 감시 모드(watch mode)를 제공합니다.

변경되는 파일들을 감시하고, 변경된 파일에 영향을 받는 테스트들만 실행합니다.

## 감시 모드를 활성화하고 테스트 실행하기

`--watch` 또는 `-w` 플래그를 사용하여 감시 모드를 활성화할 수 있습니다.

```console
$ npx ava --watch
```

감시 모드 사용 시 통합 디버깅, TAP reporter를 사용할 수 없는 것에 유의해주세요.

## 요구 사항

AVA는 [`chokidar`]를 파일 감시 라이브러리로 사용합니다.

설치하는 동안 optional dependencies가 실패한다는 경고가 표시되더라도 제대로 작동할 것 입니다.

chokidar 설치 문제를 해결하려면 `chokidar` 다큐먼트의 *[Install Troubleshooting]* 섹션을 참조하십시오.

## 변경 무시하기

기본적으로 AVA는 `snap`, `ava.config.*` 확장자의 파일을 제외한 모든 파일의 변경 사항을 감시합니다.

그 외에도 [`ignore-by-default`] 패키지에서 [certain directories](https://github.com/novemberborn/ignore-by-default/blob/master/index.js) 내의 파일들을 무시합니다.

[`package.json`, `ava.config.*` 파일의 `ava` 섹션][config]에서 `ignoredByWatcher` 키를 사용해, 무시할 파일의 패턴을 추가적으로 구성할 수 있습니다.

테스트가 디스크에 파일을 쓸 때 chokidar가 테스트를 재실행할 수 있습니다. 추가적인 무시 패턴을 설정하는 것이 이러한 문제를 방지하는데 도움을 줄 수 있습니다.

## 의존성 추적

AVA는 테스트 파일들이 의존 중인 소스 파일들을 추적합니다.

이러한 의존성을 변경하면, 이에 의존 중인 테스트 파일들만 재실행됩니다.

AVA는 어떤 테스트 파일이 변경된 소스 파일에 의존 중인지 결정할 수 없는 경우, 모든 테스트 파일을 재실행 합니다.

의존성 추적은 required된 모듈들에 대해서도 작동합니다.

[`package.json`, `ava.config.*` 파일에 설정을 추가함으로써][config] 사용자 지정 확장 및 트랜스파일러도 지원됩니다.

`fs` 모듈을 사용하여 액세스하는 파일은 추적되지 않습니다.

## 감시 모드와 `.only` 한정자

`.only` 한정자는 감시 모드의 의존성 추적 알고리즘을 비활성화 시킵니다.

변경이 감지 되었을 때, 모든 `.only` 테스트들은, 해당 테스트가 의존 중인 소스 파일에 관계 없이 재실행될 것입니다.

## 감시 모드와 CI

감시 모드로 CI에서 AVA를 실행하면 다음과 같은 에러와 함께 실행이 종료될 것입니다.

(`Error : Watch mode is not available in CI, as it prevents AVA from terminating.`). 

AVA는 CI에서 `--watch` (`-w`) 옵션과 함께 사용될 수 없습니다. CI에서 프로세스들은 종료되어야만 하고, `--watch` 옵션을 사용하면 AVA가 절대 종료되지 않을 것이기 때문입니다.

## 수동으로 모든 테스트 재실행하기

콘솔에서 <kbd>r</kbd>를 입력한 다음 <kbd>Enter</kbd>를 입력하면 모든 테스트를 빠르게 다시 실행할 수 있습니다.

## 스냅샷 업데이트

콘솔에 <kbd>u</kbd>를 입력한 다음 <kbd>Enter</kbd>를 입력하여 실패 중인(failing) 스냅샷을 업데이트할 수 있습니다.

## 디버깅

하나의 테스트만 실행될 것이라고 생각했을 때, watch 모드는 가끔 당황스럽게 모든 테스트를 재실행합니다.

AVA의 추론을 보기 위해 디버그 모드를 활성화할 수 있습니다. 이 방법은 verbose reporter에 가장 적합할 것입니다.

```console
$ DEBUG=ava:watcher npx ava --watch
```

## 감시 모드를 더 좋게 만들 수 있도록 도와주세요

감시 모드는 비교적 새로운 기능으로, 다소 조잡한 부분이 있을 수 있습니다.

당신이 만난 이슈를 [리포트](https://github.com/avajs/ava/issues) 해 주세요. 감사합니다!

[`chokidar`]: https://github.com/paulmillr/chokidar
[Install Troubleshooting]: https://github.com/paulmillr/chokidar#install-troubleshooting
[`ignore-by-default`]: https://github.com/novemberborn/ignore-by-default
[`.only` modifier]: ../01-writing-tests.md#running-specific-tests
[config]: ../06-configuration.md
