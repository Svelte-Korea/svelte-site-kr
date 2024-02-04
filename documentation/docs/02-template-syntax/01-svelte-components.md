---
title: 스벨트 컴포넌트
---

컴포넌트는 스벨트 애플리케이션의 빌딩 블록입니다. 컴포넌트는 HTML의 상위 집합을 사용하여 `.svelte` 파일로 작성됩니다.

스크립트, 스타일, 마크업의 세 가지 섹션은 선택 사항입니다.

```svelte
<script>
	// 여기에 스크립트
</script>

<!-- 여기에 마크업 (0개 이상 항목) -->

<style>
	/* 여기에 스타일 */
</style>
```

## 스크립트 (&lt;script&gt;)

`<script>` 블록에는 컴포넌트 인스턴스가 생성될 때 실행되는 자바스크립트가 포함되어 있습니다. 최상위 수준에서 선언(또는 imported)된 변수는 컴포넌트의 마크업에서 '보여'집니다. 네 가지 추가 규칙이 있습니다:

### 1. `export` 는 컴포넌트 프로퍼티를 생성함

스벨트는 `export` (내보내기) 키워드를 사용하여 변수 선언을 _property_ 또는 _prop_ 으로 표시하는데, 이는 컴포넌트의 컨슈머(소비자)가 액세스할 수 있음을 의미합니다 (자세한 내용은 [attributes and props](/docs/basic-markup#attributes-and-props) 섹션을 참조하세요).

```svelte
<script>
	export let foo;

	// prop으로 전달된 값은
	// 즉시 사용 가능
	console.log({ foo });
</script>
```

prop의 기본 초기값을 지정할 수 있습니다. 이는 컴포넌트를 인스턴스로 만들 때 컴포넌트의 컨슈머가 컴포넌트에서 prop을 지정하지 않거나 초기 값이 `undefined`인 경우 사용됩니다. 이후 프로퍼티의 값이 업데이트되면 값이 지정되지 않은 모든 prop은 초기 값이 아닌 `undefined`로 설정된다는 점에 유의하세요.

개발 모드([compiler options](/docs/svelte-compiler#compile) 참조)에서 기본 초기값이 제공되지 않고 컨슈머가 값을 지정하지 않으면 경고가 출력됩니다. 이 경고를 없애려면 `undefined`이더라도 기본 초기값이 지정되어 있는지 확인하세요.

```svelte
<script>
	export let bar = 'optional default initial value';
	export let baz = undefined;
</script>
```

`const`, `class` 또는 `function`을 내보내면 컴포넌트 외부에서 읽기 전용이 됩니다. 그러나 `function`은 아래와 같이 유효한 prop 값입니다.

```svelte
<!--- file: App.svelte --->
<script>
	// 이것은 읽기 전용입니다
	export const thisIs = 'readonly';

	/** @param {string} name */
	export function greet(name) {
		alert(`hello ${name}!`);
	}

	// 이것이 prop 입니다
	export let format = (n) => n.toFixed(2);
</script>
```

읽기 전용 프로퍼티는 요소의 프로퍼티로 액세스할 수 있으며, [`bind:this` syntax](/docs/component-directives#bind-this)을 사용하여 컴포넌트에 연결할 수 있습니다.

You can use reserved words as prop names.

```svelte
<!--- file: App.svelte --->
<script>
	/** @type {string} */
	let className;

	// `class` 프로퍼티 생성
	// (예약어이긴 하지만)
	export { className as class };
</script>
```

### 2. 할당은 '반응형'

컴포넌트 상태를 변경하고 화면을 새로 그리려면 로컬로 선언된 변수에 할당하면 됩니다.

업데이트 표현식(`count += 1`)과 프로퍼티 할당(`obj.x = y`)은 동일한 효과가 있습니다.

```svelte
<script>
	let count = 0;

	function handleClick() {
		// 마크업이 `count`를 참조하는 경우
		// 이 함수를 호출하면 업데이트를 트리거함
		count = count + 1;
	}
</script>
```

스벨트의 반응성은 할당을 기반으로 하므로 `.push()` 및 `.splice()`와 같은 배열 메서드를 사용한다고 해서 자동으로 업데이트가 트리거되지는 않습니다. 업데이트를 트리거하려면 후속 할당이 필요합니다. 이에 대한 자세한 내용은 [튜토리얼](https://learn.svelte.dev/tutorial/updating-arrays-and-objects)에서도 확인할 수 있습니다..

```svelte
<script>
	let arr = [0, 1];

	function handleClick() {
		// 이 메서드 호출은 업데이트를 트리거하지 않음
		arr.push(2);
		// 마크업이 `arr`를 참조하는 경우
		// 이 할당에서 업데이트를 트리거함
		arr = arr;
	}
</script>
```

스벨트의 `<script>` 블록은 컴포넌트가 생성될 때만 실행되므로, `<script>` 블록 내의 할당은 컴포넌트가 업데이트될 때 자동으로 다시 실행되지 않습니다. 컴포넌트의 변경 사항을 추적하려면 다음 섹션의 예시를 참조하세요.

```svelte
<script>
	export let person;
	// 컴포넌트 생성 시에만 `name` 설정
	// `person`이 생성될 때는 업데이트되지 않음
	let { name } = person;
</script>
```

### 3. `$:` 를 사용해 스테이트먼트를 반응형으로 표시

블록이나 함수 내부가 아닌 최상위 스테이트먼트(문) 앞에 `$:` [JS label syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/label)을 추가하여 반응형 스테이트먼트로 만들 수 있습니다. 반응형 스테이트먼트는 다른 스크립트 코드 다음에 컴포넌트 마크업이 렌더링되기 전에 해당 스테이트먼트가 의존하는 값이 변경될 때마다 실행됩니다.

```svelte
<script>
	export let title;
	export let person;

	// `title`프로퍼티가 변경될 때마다
	// `document.title`을 업데이트
	$: document.title = title;

	$: {
		console.log(`multiple statements can be combined`);
		console.log(`the current title is ${title}`);
	}

	// `person`이 변경될 때 `name`을 업데이트
	$: ({ name } = person);

	// 이렇게 하지 말 것. 이 줄은 이전 줄이 실행되기 전에 실행됨
	let name2 = name;
</script>
```

블록 `$:` 내에 직접 나타나는 값만 반응형 스테이트먼트에 종속됩니다. 예를 들어, 아래 코드에서 `total`은 `x`가 변경될 때만 업데이트되고 `y`는 업데이트되지 않습니다.

```svelte
<!--- file: App.svelte --->
<script>
	let x = 0;
	let y = 0;

	/** @param {number} value */
	function yPlusAValue(value) {
		return value + y;
	}

	$: total = yPlusAValue(x);
</script>

Total: {total}
<button on:click={() => x++}> Increment X </button>

<button on:click={() => y++}> Increment Y </button>
```

반응형 블록은 컴파일 시 간단한 정적 분석을 통해 정렬되며, 컴파일러는 블록이 호출하는 함수가 아닌 블록 자체에 할당되고 사용되는 변수만 살펴본다는 점에 유의해야 합니다. 즉, 다음 예제에서 `x`가 업데이트될 때 `yDependent`는 업데이트되지 않습니다:

```svelte
<script>
	let x = 0;
	let y = 0;

	/** @param {number} value */
	function setY(value) {
		y = value;
	}

	$: yDependent = y;
	$: setY(x);
</script>
```

`yDependent = y` 줄을 `$: setY(x)` 아래로 옮기면 `x`가 업데이트될 때 `yDependent`가 업데이트됩니다.

명령문이 선언되지 않은 변수에 대한 대입으로만 구성된 경우, 스벨트는 사용자를 대신해 `let` 선언을 삽입합니다.

```svelte
<!--- file: App.svelte --->
<script>
	/** @type {number} */
	export let num;

	// `squared`과 `cubed`를 선언(let)할 필요 없음
	// — 스벨트가 대신 해 줌
	$: squared = num * num;
	$: cubed = squared * num;
</script>
```

### 4. `$`를 스토어 앞에 붙여서 그 값에 접근

_store_는 간단한 _store contract_를 통해 값에 대한 반응형 액세스를 허용하는 객체입니다. [`svelte/store` module](/docs/svelte-store) 에는 이 컨트랙트를 이행하는 최소한의 저장소 구현이 포함되어 있습니다.

스토어에 대한 참조가 있을 때마다 컴포넌트 내부에서 `$` 문자를 접두사로 붙여서 해당 값에 액세스할 수 있습니다. 이렇게 하면 스벨트가 접두사가 붙은 변수를 선언하고 컴포넌트 초기화 시 스토어에 구독하고 적절한 경우 구독을 취소합니다.

접두사가 `$`인 변수에 할당하려면 해당 변수가 쓰기 가능한 스토어여야 하며, 저장소의 `.set` 메서드를 호출하게 됩니다.

스토어는 `if` 블록이나 함수 내부가 아닌 컴포넌트의 최상위 레벨에서 선언해야 합니다.

(스토어 값을 나타내지 않는) 로컬 변수에는 `$` 접두사가 _없어야_ 합니다.

```svelte
<script>
	import { writable } from 'svelte/store';

	const count = writable(0);
	console.log($count); // logs 0

	count.set(1);
	console.log($count); // logs 1

	$count = 2;
	console.log($count); // logs 2
</script>
```

#### 스토어 컨트랙트 (Store contract)

```ts
// @noErrors
store = { subscribe: (subscription: (value: any) => void) => (() => void), set?: (value: any) => void }
```

스토어 컨트랙트_를 구현하여 [`svelte/store`](/docs/svelte-store)에 의존하지 않고도 자체 스토어를 만들 수 있습니다:

1. 스토어에는 구독 함수를 인수로 받는 '.subscribe' 메서드가 포함되어야 합니다. 이 구독 함수는 '.subscribe'를 호출할 때 스토어의 현재 값과 함께 즉시 동기적으로 호출되어야 합니다. 나중에 스토어 값이 변경될 때마다 스토어의 모든 활성 구독 함수가 동기적으로 호출되어야 합니다.
2. .subscribe` 메서드는 구독 취소 함수를 반환해야 합니다. 구독 취소 함수를 호출하면 구독이 중지되어야 하며, 스토어에서 해당 구독 함수를 다시 호출하지 않아야 합니다.
3. 스토어는 스토어에 대한 새 값을 인수로 받아들여야 하며 스토어의 모든 활성 구독 함수를 동기적으로 호출하는 '.set' 메서드를 _선택적으로_ 포함할 수 있습니다. 이러한 스토어를 _쓰기 가능한 스토어_라고 합니다.

RxJS 옵저버블과의 상호 운용성을 위해 `.subscribe` 메서드는 구독 취소 함수를 직접 반환하는 대신 `.unsubscribe` 메서드가 있는 객체를 반환하는 것도 허용됩니다. 그러나 `.subscribe`가 구독을 동기적으로 호출하지 않는 한(옵저버블 사양에서 요구하지 않음), 스벨트는 구독을 호출할 때까지 저장소의 값을 `undefined`로 간주합니다.

## &lt;script context="module"&gt;

`context="module"` 속성이 있는 `<script>` 태그는 각 컴포넌트 인스턴스마다 실행되는 것이 아니라 모듈이 처음 평가될 때 한 번 실행됩니다. 이 블록에 선언된 값은 일반 `<script>`(및 컴포넌트 마크업)에서 액세스할 수 있지만 그 반대의 경우는 불가능합니다.

이 블록에서 바인딩을 `export`할 수 있으며, 컴파일된 모듈의 내보내기가 됩니다.

기본 내보내기는 컴포넌트 자체이므로 `export default`는 불가능합니다.

> `module` 스크립트에 정의된 변수는 반응형 변수가 아니므로 변수를 재할당해도 변수 자체는 업데이트되더라도 다시 렌더링되지 않습니다. 여러 컴포넌트 간에 공유되는 값의 경우, [store](/docs/svelte-store)를 사용하는 것이 좋습니다.

```svelte
<script context="module">
	let totalComponents = 0;

	// export 키워드를 사용하면 이 함수를 다음처럼 가져올 수 있음
	// `import Example, { alertTotal } from './Example.svelte'`
	export function alertTotal() {
		alert(totalComponents);
	}
</script>

<script>
	totalComponents += 1;
	console.log(`total number of times this component has been created: ${totalComponents}`);
</script>
```

## &lt;style&gt;

`<style>` 블록 안의 CSS는 해당 컴포넌트로 범위가 지정됩니다.

이는 영향을 받는 요소에 컴포넌트 스타일의 해시(예: `svelte-123xyz`)를 기반으로 하는 클래스를 추가하는 방식으로 작동합니다.

```svelte
<style>
	p {
		/* 이 컴포넌트의 <p> 요소에만 영향을 미침 */
		color: burlywood;
	}
</style>
```

셀렉터에 스타일을 전역적으로 적용하려면 `:global(...)` 모디파이어를 사용합니다.

```svelte
<style>
	:global(body) {
		/* <body>에 적용 */
		margin: 0;
	}

	div :global(strong) {
		/* 이 컴포넌트에 속한
			<div> 요소 안에 있는
			모든 <strong> 요소에 적용 */
		color: goldenrod;
	}

	p:global(.red) {
		/* 이렇게 설정하면 마크업에 처음에
			 class="red"가 나타나지 않고 런타임에
			 추가되더라도 class="red"인 이 컴포넌트에
			 속한 모든 <p> 요소에 적용됩니다. 이는 요소의
			 classList 속성을 직접 업데이트하는 등
			 요소의 클래스가 동적으로 적용될 때 유용합니다. */
	}
</style>
```

전역적으로 액세스할 수 있는 @keyframes를 만드려면 키프레임 이름 앞에 `-global-`을 추가해야 합니다.

컴파일할 때 `-global-` 부분이 제거되고 코드의 다른 곳에서 `my-animation-name`만 사용하여 키프레임을 참조할 수 있습니다.

```svelte
<style>
	@keyframes -global-my-animation-name {
		/* 여기에 코드 */
	}
</style>
```

컴포넌트당 최상위 `<style>` 태그는 1개만 있어야 합니다.

그러나 다른 요소나 로직 블록 안에 `<style>` 태그를 중첩할 수 있습니다.

이 경우 `<style>` 태그는 DOM에 그대로 삽입되며, `<style>` 태그에 대한 스코핑이나 처리는 수행되지 않습니다.

```svelte
<div>
	<style>
		/* this style tag will be inserted as-is */
		div {
			/* this will apply to all `<div>` elements in the DOM */
			color: red;
		}
	</style>
</div>
```
