<script lang="ts">
	import CardHover from '$lib/components/CardHover.svelte';
	import { onMount, onDestroy } from 'svelte';

	type CharacterState = 'front' | 'left' | 'right';

	let cursorX: number = 0;
	let lastCursorX: number = 0;

	let characterX: number = 50;
	const characterY: number = 0;

	let characterState: CharacterState = 'front';

	const characterImages: Record<CharacterState, string> = {
		front: '/images/character/c-front.svg',
		left: '/images/character/c-left.svg',
		right: '/images/character/c-right.svg'
	};

	function handleMouseMove(event: MouseEvent): void {
		cursorX = (event.clientX / window.innerWidth) * 100;

		if (cursorX > lastCursorX) {
			characterState = 'right';
		} else if (cursorX < lastCursorX) {
			characterState = 'left';
		}

		characterX = cursorX;

		lastCursorX = cursorX;
	}

	function handleMouseStop(): void {
		characterState = 'front';
	}

	onMount(() => {
		window.addEventListener('mousemove', handleMouseMove);

		const stopInterval = setInterval(() => {
			if (cursorX === lastCursorX) {
				handleMouseStop();
			}
		}, 1000);

		onDestroy(() => {
			window.removeEventListener('mousemove', handleMouseMove);
			clearInterval(stopInterval);
		});
	});
</script>

<div
	class="relative mx-auto h-screen w-full overflow-hidden bg-cover bg-no-repeat p-1 text-white"
	style="background-image: url('/images/hero.png'); background-size: cover; background-position: center;"
>
	<CardHover />

	<img
		src={characterImages[characterState]}
		alt="Character"
		class="absolute z-10 transition-transform duration-100 ease-in"
		{...{
			style: `left: ${characterX}vw; bottom: -90px; height:80vh `
		}}
	/>

	<div
		class="absolute bottom-0 left-0 z-30 h-[20vw] w-full bg-gradient-to-br from-[#CC9C49]/10 to-[#CC9C49]/45 mask-t-from-10%"
	></div>

	<img src="/images/fence.svg" alt="Fence" class="absolute bottom-0 left-0 z-20 w-full" />

	<div class="absolute right-0 bottom-0 z-20 mb-14 px-5 md:mb-20 md:px-10">
		<h1 class="mb-1 text-2xl font-bold md:mb-3 md:text-6xl">Movei asd sad sad asd sad sar</h1>
		<p class="mb-2 text-sm md:mb-5 md:text-lg">
			Lorem ipsum dolor sit amet consectetur adipisicing elit. Culpa odio nesciunt vel, quae eum
			esse quibusdam voluptatem fugit voluptatibus modi odit nostrum ipsam hic doloribus cum. Quam
			reprehenderit architecto fuga?
		</p>
	</div>
</div>

<h1>
	Lorem ipsum dolor sit, amet consectetur adipisicing elit. Inventore recusandae, deserunt delectus
	cumque pariatur voluptate enim iusto eveniet iste, eaque dolore quidem iure magnam nostrum,
	tempora similique atque blanditiis voluptates?
</h1>
