[![Cybernetically enhanced web apps: Svelte](https://sveltejs.github.io/assets/banner.png)](https://svelte.dev)

[![license](https://img.shields.io/npm/l/svelte.svg)](LICENSE.md) [![Chat](https://img.shields.io/discord/457912077277855764?label=chat&logo=discord)](https://svelte.dev/chat)

## 스벨트란 무엇인가요?

스벨트는 웹 애플리케이션을 구축하는 새로운 방법입니다. 스벨트는 선언적 컴포넌트를 가져와서 DOM을 효율적으로 업데이트하는 자바스크립트로 변환하는 컴파일러입니다.

자세한 내용은 [스벨트 한국 사이트](https://svelte.kr)에서 확인하거나. [디스코드 채팅방](https://svelte.dev/chat)을 방문해서 알아보세요.

## 스벨트 후원하기

스벨트는 MIT 라이선스의 오픈 소스 프로젝트로, 훌륭한 자원봉사자들에 의해 지속적인 개발이 이루어지고 있습니다. 자원봉사자들의 노력에 힘을 보태고 싶으시다면 참여해 주세요:

- [Open Collective의 후원자 되기](https://opencollective.com/svelte).

Open Collective를 통해 기부된 기금은 호스팅 비용 등 스벨트의 개발과 관련된 비용을 보전하는 데 사용됩니다. 충분한 기부금이 모일 경우, 스벨트의 개발을 보다 직접적으로 지원하기 위해 기금을 사용할 수도 있습니다.

## 로드맵

현재 진행 중인 작업을 확인하려면 [로드맵](https://svelte.dev/roadmap)을 참조하세요.

## 기여

스벨트에 기여하는 방법은 [기여 가이드](CONTRIBUTING.md) 및 [스벨트 패키지](packages/svelte)를 참조하세요.

### 개발

풀 리퀘스트는 언제나 환영입니다. [이슈를 선택하고](https://github.com/sveltejs/svelte/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc) 우리를 도와주세요!

로컬에서 스벨트를 설치하고 작업하기 위해 다음처럼 진행하세요:

```bash
git clone https://github.com/sveltejs/svelte.git
cd svelte
pnpm install
```

> 종속성을 설치하는 데에 Yarn을 사용하지 마세요. `pnpm-lock.json` 의 특정 패키지 버전을 사용하여 스벨트를 빌드하고 테스트하고 있습니다.

컴파일러와 패키지에 포함된 다른 모든 모듈을 빌드하기 위해 다음처럼 진행하세요:

```bash
pnpm build
```

변경 사항을 확인하고 지속적으로 패키지를 다시 빌드하기 위해 다음처럼 진행하세요. ([`pnpm link`](https://pnpm.io/cli/link)를 사용하여 프로젝트의 변경 사항을 로컬에서 테스트하는 경우 유용합니다):

```bash
pnpm dev
```

컴파일러는 자바스크립트로 작성되었으며 [JSDoc](https://jsdoc.app/index.html) 주석을 사용하여 타입을 검사합니다.

### 실행 테스트

```bash
pnpm test
```

테스트를 필터링하려면 `-g`(`--grep`)를 사용합니다. 예를 들어 트랜지션과 관련된 테스트만 실행하려면 다음처럼 합니다:

```bash
pnpm test -- -g transition
```

### svelte.dev

https://svelte.dev 의 소스 코드는 [sites](https://github.com/sveltejs/svelte/tree/master/sites/svelte.dev) 폴더에 있으며, 모든 문서는 바로 [여기](https://github.com/sveltejs/svelte/tree/master/documentation)에 있습니다. 이 사이트는 [스벨트킷](https://kit.svelte.dev)으로 구축되었습니다.

> 번역자 주: 한국 문서의 소스 코드는 [sites](https://github.com/Svelte-Korea/svelte-site-kr/tree/main-kr/sites) 폴더에 있으며, 모든 문서는 [documentation](https://github.com/Svelte-Korea/svelte-site-kr/tree/main-kr/documentation) 폴더에 있습니다. 이 사이트는 https://svelte.dev 의 구조를 따르며 번역을 진행합니다.

## svelte.dev에 접속되지 않나요?

그럴 리는 없겠지만 그럴 수도 있습니다. '.dev' 사이트에 액세스할 수 없는 것 같으면 [이 슈퍼유저 질문과 답변](https://superuser.com/q/1413402)을 확인하세요.

## 라이센스

[MIT](LICENSE.md)
