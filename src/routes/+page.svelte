<script lang="ts">
	import CardHover from '$lib/components/CardHover.svelte';
	import { onMount, onDestroy } from 'svelte';

	type CharacterState = 'front' | 'left' | 'right';

	let cursorX = 0;
	let lastCursorX = 0;

	let characterX = 50;
	let characterState: CharacterState = 'front';

	const characterImages: Record<CharacterState, string> = {
		front: '/images/character/c-front.svg',
		left: '/images/character/c-left.svg',
		right: '/images/character/c-right.svg'
	};

	let isMobile = false;
	let scrollContainer: HTMLElement;

	function handleMouseMove(event: MouseEvent): void {
		if (isMobile) return; // disable on mobile

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
		if (!isMobile) {
			characterState = 'front';
		}
	}

	function handleScroll(): void {
		if (!scrollContainer) return;

		const scrollLeft = scrollContainer.scrollLeft;
		const containerWidth = scrollContainer.clientWidth;

		// Perubahan arah untuk karakterState
		if (scrollLeft > lastCursorX) {
			characterState = 'right';
		} else if (scrollLeft < lastCursorX) {
			characterState = 'left';
		}

		lastCursorX = scrollLeft;

		// Posisi karakter adalah posisi scroll + tengah layar (jadi tetap mengikuti bagian tengah background image)
		characterX = scrollLeft + containerWidth / 2;
	}

	onMount(() => {
		isMobile = window.innerWidth < 768;

		if (isMobile) {
			scrollContainer?.addEventListener('scroll', handleScroll);
		} else {
			window.addEventListener('mousemove', handleMouseMove);
		}

		const stopInterval = setInterval(() => {
			handleMouseStop();
		}, 1000);

		onDestroy(() => {
			window.removeEventListener('mousemove', handleMouseMove);
			scrollContainer?.removeEventListener('scroll', handleScroll);
			clearInterval(stopInterval);
		});
	});
</script>

<!-- <div
	class="relative mx-auto h-screen w-full overflow-hidden bg-cover bg-no-repeat p-1 text-white"
	style="background-image: url('/images/hero.png'); background-size: cover; background-position: center;"
> -->

<div
	class="relative h-[712px] w-full overflow-x-auto overflow-y-hidden bg-black md:h-screen md:overflow-hidden"
	bind:this={scrollContainer}
>
	<!-- Inner div -->
	<div
		class="relative mx-auto h-full min-w-[1200px] bg-[length:auto] bg-center bg-no-repeat p-1 text-white md:min-w-full md:bg-cover md:bg-center"
		style="background-image: url('/images/hero.png');"
	>
		<CardHover />

		<img
			src={characterImages[characterState]}
			alt="Character"
			class="absolute bottom-[-150px] left-0 z-10 h-[60vh] transition-transform duration-100 ease-in md:bottom-[-90px] md:h-[80vh]"
			{...{
				style: isMobile ? `transform: translateX(${characterX}px);` : `left: ${characterX}vw;`
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
</div>

<h1>
	Lorem ipsum dolor sit, amet consectetur adipisicing elit. Inventore recusandae, deserunt delectus
	cumque pariatur voluptate enim iusto eveniet iste, eaque dolore quidem iure magnam nostrum,
	tempora similique atque blanditiis voluptates?
</h1>
