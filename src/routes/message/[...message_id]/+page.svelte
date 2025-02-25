<script lang="ts">
	import { onMount } from 'svelte';
	import Typewriter from 'svelte-typewriter';

	let isLetterOpen: boolean = $state(false);
	let showHeart: boolean = $state(false);
	let letterElement: HTMLDivElement | null = $state(null);
	let heartButton: HTMLButtonElement | null = $state(null);
	let videoElement: HTMLVideoElement;

	onMount(() => {
		if (videoElement) {
			videoElement.play();
			videoElement.addEventListener('ended', handleVideoEnd);
		}
		document.addEventListener('click', handleClickOutside);

		// Cleanup
		return () => {
			if (videoElement) {
				videoElement.removeEventListener('ended', handleVideoEnd);
			}
			document.removeEventListener('click', handleClickOutside);
		};
	});

	function handleVideoEnd(): void {
		showHeart = true;
	}

	function handleClickOutside(event: MouseEvent): void {
		const target = event.target as Node;
		// Check if click is outside both the letter and the heart button
		if (
			isLetterOpen &&
			letterElement &&
			!letterElement.contains(target) &&
			heartButton &&
			!heartButton.contains(target)
		) {
			isLetterOpen = false;
		}
	}

	function handleHeartClick(event: MouseEvent): void {
		event.stopPropagation();
		isLetterOpen = true;
	}
</script>

<main
	class="main relative grid size-full h-svh place-content-center place-items-center bg-red-500/10"
>
	<div class="box relative max-w-[350px]">
		<video
			bind:this={videoElement}
			class="vid size-full bg-inherit object-contain object-center mix-blend-multiply"
			playsInline
			autoPlay={false}
			muted={false}
			loop={false}
			controls={false}
		>
			<source src="/bear-letter-open.mp4" type="video/mp4" />
		</video>

		{#if showHeart}
			<button
				bind:this={heartButton}
				class="letter-click absolute bottom-[34px] grow translate-x-[159px] -translate-y-1/2 cursor-pointer"
				onclick={handleHeartClick}
			>
				<svg
					width="50"
					height="50"
					viewBox="0 0 200 200"
					fill="none"
					xmlns="http://www.w3.org/2000/svg"
					class="transition-transform duration-300 hover:scale-110"
				>
					<path
						d="M100 170C80 150 30 110 30 65C30 35 55 20 75 20C90 20 100 35 100 35C100 35 110 20 125 20C145 20 170 35 170 65C170 110 120 150 100 170Z"
						fill="#FF4F92"
						stroke="#5A1E12"
						stroke-width="15"
						stroke-linecap="round"
						stroke-linejoin="round"
					/>
				</svg>
				<span class="letter-text sr-only text-3xl text-white">Open Letter</span>
			</button>
		{/if}
	</div>

	{#if isLetterOpen}
		<div
			class="layer-grap fixed inset-0 z-5 bg-black/50 backdrop-blur-sm transition-all duration-300"
		></div>
		<div
			bind:this={letterElement}
			class="letter scale-in-center absolute top-1/2 left-1/2 z-10 flex h-[90svh] max-h-[640px] min-h-54 w-full max-w-sm -translate-1/2 flex-col items-center justify-center gap-1 overflow-hidden rounded-lg bg-[#f9c0dc] p-6 text-base shadow-lg transition-all duration-300"
		>
			<div class="box mt-4 max-h-[calc(100%_-_120px)] overflow-y-auto">
				<div class="inner-box flex size-full flex-col gap-1 select-none">
					<div class="header text-center">
						<h3 class="oxygen-bold title text-2xl font-bold">Will You Be Mine?</h3>
					</div>
					<div class="oxygen-regular body space-y-2 text-center text-[18px] tracking-wider">
						<Typewriter wordInterval={50000} mode={'cascade'} cursor={false}></Typewriter>
						<p>
							From the moment our paths crossed, I've felt a connection that words struggle to
							capture. Your smile brightens even my darkest days, and your laughter has become my
							favorite melody.
						</p>
						<p>
							The thought of building memories together fills me with joy I never knew possible. So
							I'm gathering my courage to ask... would you be my valentine, not just for a day, but
							for all the days to come?
						</p>
					</div>
					<div class="mt-3 flex items-center justify-center gap-5">
						<button
							class="btn-green cursor-pointer rounded-sm px-6 py-1.5 font-medium hover:bg-gray-500/4"
						>
							Yes
						</button>
						<button
							class="btn-red cursor-pointer rounded-sm px-6 py-1.5 font-medium hover:bg-gray-500/4"
						>
							No
						</button>
					</div>
				</div>
			</div>
		</div>
	{/if}
</main>

<style>
	.letter::after {
		top: -78px;
		bottom: 100px;
		left: 0;
		right: 0;
		width: 100%;
		height: 116%;
		position: absolute;
		content: '';
		background-size: cover;
		background-repeat: no-repeat;
		opacity: 0.45;
		z-index: -5;

		background-image: url('/bg-background.png');
	}

	.scale-in-center {
		animation: scale-in-center 0.3s cubic-bezier(0.25, 0.46, 0.45, 0.94) both;
	}

	@keyframes scale-in-center {
		0% {
			transform: scale(0);
			opacity: 0;
		}
		100% {
			transform: scale(1);
			opacity: 1;
		}
	}

	.grow {
		animation: grow-in 1.5s infinite cubic-bezier(0.25, 0.46, 0.45, 0.94) both;
	}

	@keyframes grow-in {
		0% {
			transform: scale(0.8);
		}
		100% {
			transform: scale(1.2);
			opacity: 1;
		}
	}
</style>
