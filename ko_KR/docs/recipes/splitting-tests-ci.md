___
**역자주**

이 문서는 [splitting-tests-ci.md](https://github.com/avajs/ava/blob/main/docs/recipes/splitting-tests-ci.md)의 한국어 번역입니다. [이곳](https://github.com/avajs/ava/compare/71404c23302d825095659c70cb9a1b08251697ad...main#diff-0730bb7c2e8f9ea2438b52e419dd86c9)에서 AVA의 master 브랜치와 이 문서의 차이를 확인할 수 있습니다. (만약 차이가 없다면 문서가 최신 버전임을 의미합니다)
___

# CI에서 테스트 나누기

AVA는 [ci-parallel-vars](https://www.npmjs.com/package/ci-parallel-vars)를 사용하여 자동으로 당신의 CI 환경이 병렬 빌드(parallel build)를 지원하는지 감지합니다.
병렬 빌드가 지원되는 경우, AVA는 모든 테스트 파일들의 이름을 정렬하고, 그것들을 청크들(chunks)로 나눕니다. 각 CI 머신들은 테스트의 부분 집합들을 할당 받고, 각 청크 (테스트) 들은 병렬로 실행됩니다.

테스트를 시스템 전체에 더 잘 분산하기 위해 comparator을 아래처럼 커스터마이징 할 수 있습니다.

**`ava.config.js`:**

```js
import fs from 'node:fs';

// 'test-data.json' 파일의 구조가 아래와 같다고 가정:
// {
// 	'tests/test1.js': { order: 1 },
// 	'tests/test2.js': { order: 0 }
// }
const testData = JSON.parse(fs.readFileSync('test-data.json', 'utf8'));

export default {
	sortTestFiles: (file1, file2) => testData[file1].order - testData[file2].order,
};
```
