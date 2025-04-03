<script lang="ts">
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
</script>

<nav
	class="sticky top-0 left-0 z-50 flex items-center justify-between border bg-transparent p-3 backdrop-blur transition-all duration-300 md:px-10"
>
	<a href="/" class="font-natumemozi text-2xl" aria-label="icon">
		<span class="text-primary font-light">MAdventure 映画の冒険</span>
	</a>

	<div class="flex justify-between gap-5">
		{#each link as { href, title }}
			<a {href}>{title}</a>
		{/each}
	</div>
</nav>
