___
**역자주**

이 문서는 [debugging-with-chrome-devtools.md](https://github.com/avajs/ava/blob/main/docs/recipes/debugging-with-chrome-devtools.md)의 한국어 번역입니다. [이곳](https://github.com/avajs/ava/compare/71404c23302d825095659c70cb9a1b08251697ad...main#diff-0730bb7c2e8f9ea2438b52e419dd86c9)에서 AVA의 master 브랜치와 이 문서의 차이를 확인할 수 있습니다. (만약 차이가 없다면 문서가 최신 버전임을 의미합니다)
___

# Chrome DevTools로 테스트 디버깅하기

[Chrome DevTools](https://developers.google.com/web/tools/chrome-devtools)를 사용하여 테스트를 디버그할 수 있습니다.

Chrome을 열고, <chrome://inspect/>로 이동하세요.

*Devices* 섹션의 *Open dedicated DevTools for Node* 링크를 클릭하세요.

*DevTools for Node* 창에서, *Sources*로 이동하고, 왼쪽 열들 중 *Filesystem*을 클릭하세요.

작업 영역에 프로젝트 디렉토리를 추가하세요. 적절한 권한을 부여해야 함을 유의하세요.

이제 특정 테스트 파일을 실행합니다.

```console
npx ava debug test.js
```

DevTools가 자동으로 연결되고 당신의 테스트가 실행될 것입니다.

DevTools를 사용하여 중단점을 설정하거나 `debugger` 키워드를 사용하세요.

테스트 파일이 로드되기 바로 전에 DevTools가 중단점에 도달하도록 하려면 `--break` 옵션과 함께 실행하세요.

```console
npx ava debug --break test.js
```

기본적으로 inspector는 `127.0.0.1:9229`에서 대기(listens on)합니다.

호스트와 포트를 사용자 정의할 수 있습니다.

```console
npx ava debug --host 0.0.0.0 --port 9230 test.js
```

*Connection* 탭에서 이 포트에 대한 연결을 추가해야 합니다.
