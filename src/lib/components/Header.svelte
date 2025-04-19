<script lang="ts">
	import { onMount } from 'svelte';

	let link = [
		{
			title: 'About',
			href: '/about'
		},
		{
			title: 'Movie',
			href: '/movie'
		},
		{
			title: 'Galery',
			href: '/galery'
		},
		{
			title: 'List',
			href: '/list'
		}
	];

	let isScrolled = false;

	onMount(() => {
		const handleScroll = () => {
			isScrolled = window.scrollY > 10;
		};

		window.addEventListener('scroll', handleScroll);
		handleScroll(); // Trigger langsung

		return () => window.removeEventListener('scroll', handleScroll);
	});
</script>

<nav
	class="sticky top-0 left-0 z-50 flex items-center justify-between bg-transparent p-3 backdrop-blur transition-all duration-300 md:px-10"
	class:text-white={isScrolled}
	class:text-black={!isScrolled}
>
	<a href="/" class="font-natumemozi text-2xl" aria-label="icon">
		<span class="text-primary font-light">MAdventure 映画の冒険</span>
	</a>

	<div class="hidden justify-between gap-5 md:flex">
		{#each link as { href, title }}
			<a {href}>{title}</a>
		{/each}
	</div>
</nav>
