___
**역자주**

이 문서는 [04-snapshot-testing.md](https://github.com/avajs/ava/blob/main/docs/04-snapshot-testing.md)의 한국어 번역입니다. [이곳](https://github.com/avajs/ava/compare/71404c23302d825095659c70cb9a1b08251697ad...main#diff-0730bb7c2e8f9ea2438b52e419dd86c9)에서 AVA의 master 브랜치와 이 문서의 차이를 확인할 수 있습니다. (만약 차이가 없다면 문서가 최신 버전임을 의미합니다)
___

# 스냅샷 테스팅

AVA는 [Jest에서 도입된 스냅샷 테스팅](https://facebook.github.io/jest/docs/snapshot-testing.html)을 지원합니다.

해당 기능은 [단언문 인터페이스](./03-assertions.md)를 통해 사용할 수 있습니다. 어떤 값이든 스냅샷을 찍어 놓을 수 있습니다.

스냅샷은 테스트 파일과 함께 저장됩니다.

테스트 파일들이 `test`나, `tests` 폴더에 있다면, 스냅샷들은 `snapshots` 폴더에 저장됩니다.

테스트 파일들이 `__tests__` 폴더에 있다면, 스냅샷들은 `__snapshots__` 폴더에 저장됩니다.

스냅샷 단언문을 포함하는 `~/project/test/main.js` 테스트 파일이 있다고 가정해봅시다.

AVA는 두 개의 파일을 만들 것 입니다:

* `~/project/test/snapshots/main.js.snap`
* `~/project/test/snapshots/main.js.md`

첫 번째 파일은 실질적인 스냅샷 파일로, 미래에 비교를 위해 사용됩니다.

두 번째 파일은 *스냅샷 리포트*를 포함합니다.

이 파일은 스냅샷이 업데이트 될 때 재생성됩니다.

만약 소스 컨트롤에 커밋한다면, 해당 파일로 스냅샷에 대한 변경 사항을 비교해 볼 수 있습니다.

AVA는 스냅샷 단언문이 실패한 이유를 보여줍니다. :

<img src="https://raw.githubusercontent.com/avajs/ava/main/media/snapshot-testing.png" width="1048">

그럼 코드를 확인해보면 됩니다.

만약 스냅샷 파일들의 변화가 의도된 것이라면, `--update-snapshots` (또는 `-u`) 플래그를 사용해 스냅샷들을 업데이트 할 수 있습니다.

```console
$ ava --update-snapshots
```

만약 특정 테스트들을 위해 스냅샷을 업데이트 할 필요가 있다면, `--update-snapshots` 플래그와 함께 `--match` 플래그를 사용하거나 `.only()`를 사용해 테스트를 선택하세요.

[`package.json` 설정](./06-configuration.md)에 스냅샷 파일들을 저장할 위치를 지정할 수 있습니다.

**`package.json`:**

```json
{
	"ava": {
		"snapshotDir": "custom-directory"
	}
}
```

스냅샷 파일은 테스트 파일들의 경로를 반영한 디렉터리 구조에 저장될 것입니다.

precompile된 테스트 파일에 대해 AVA를 실행하는 경우, AVA는 원본 파일들의 위치를 확인하기 위해 소스 맵(source map)을 사용하려 할 것입니다.

스냅샷 파일들은 해당 소스 맵 파일들과 함께 저장될 것입니다. 이 때에도 AVA가 직접 원본 파일들을 실행했을 때와 같은 룰을 따릅니다.

이것은 타입스크립트로 테스트를 작성할 때 아주 유용합니다. (참조: [타입스크립트 레시피](./recipes/typescript.md))
