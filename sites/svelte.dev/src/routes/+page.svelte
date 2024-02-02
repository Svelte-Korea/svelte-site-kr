<script>
	import { Blurb, Footer, TrySection } from '@sveltejs/site-kit/home';
	import Demo from './_components/Demo.svelte';
	import Hero from './_components/Hero.svelte';
	import Supporters from './_components/Supporters/index.svelte';
	import WhosUsingSvelte from './_components/WhosUsingSvelte/index.svelte';
</script>

<svelte:head>
	<title>Svelte • Cybernetically enhanced web apps</title>

	<meta name="twitter:title" content="Svelte" />
	<meta name="twitter:description" content="Cybernetically enhanced web apps" />
	<meta name="Description" content="Cybernetically enhanced web apps" />
</svelte:head>

<h1 class="visually-hidden">Svelte</h1>

<Hero />

<Blurb --background="var(--sk-back-1)">
	<div slot="one">
		<h2>compiled</h2>
		<p>
			Svelte는 최대한 많은 작업을 브라우저가 아닌 빌드 단계에서 수행합니다. 더 이상 더 이상 수동 최적화가 필요 없습니다.
			더 빠르고 효율적인 앱만 있으면 됩니다.
		</p>
	</div>

	<div slot="two">
		<h2>compact</h2>
		<p>
			이미 알고 있는 언어인 HTML, CSS, 자바스크립트를 사용하여 놀랍도록 간결한 컴포넌트를 작성하세요.
			오, 그리고 애플리케이션 번들의 크기도 작아집니다.
		</p>
	</div>

	<div slot="three">
		<h2>complete</h2>
		<p>
			범위 지정 스타일링, 상태 관리, 모션 프리미티브, 폼 바인딩 등이 내장되어 있습니다.
			npm을 찾느라 시간을 낭비하지 마세요. 모든 것이 여기에 있습니다.
		</p>
	</div>
</Blurb>

<TrySection />

<Demo />

<WhosUsingSvelte />

<Supporters />

<Footer
	links={{
		resources: [
			{
				title: 'documentation',
				href: '/docs'
			},
			{
				title: 'tutorial',
				href: '/tutorial'
			},
			{
				title: 'examples',
				href: '/examples'
			},
			{
				title: 'blog',
				href: '/blog'
			}
		],
		connect: [
			{
				title: 'github',
				href: 'https://github.com/sveltejs/svelte'
			},
			{
				title: 'opencollective',
				href: 'https://opencollective.com/svelte'
			},
			{
				title: 'discord',
				href: '/chat'
			},
			{
				title: 'twitter',
				href: 'https://twitter.com/sveltejs'
			}
		]
	}}
>
	<span slot="license">
		Svelte is <a href="https://github.com/sveltejs/svelte">free and open source software</a> released
		under the MIT license
	</span>
</Footer>

<style>
	h2 {
		line-height: 1.05;
	}

	p {
		font-size: var(--sk-text-m);
	}
</style>
