<script>
	import LoveBg from '../assets/love-bg-black.svg';
</script>

<main
	class="main animate-fade relative grid size-full h-svh place-content-center place-items-center bg-red-500/10 p-4 sm:p-6"
>
	<div
		class="box-in relative z-10 flex w-full max-w-3xl flex-col items-center justify-center gap-2 overflow-hidden rounded-xl bg-white/96 p-6 shadow-xl sm:w-lg md:p-8"
	>
		<h1 class="text-center text-6xl font-bold text-red-500/90 italic">Love Craft</h1>
		<p class="oxygen-semibold text-center text-lg">
			Love Craft is a heartfelt platform to create and send personalized love notes. Choose a
			beautiful template, write your message, and deliver it via email or a shareable link.
		</p>
		<a
			href="/craft"
			class="go-to mt-2 block cursor-pointer rounded-lg bg-red-500 p-2.5 px-6 font-bold tracking-widest text-white uppercase transition-shadow hover:shadow-sm hover:shadow-red-500/50"
		>
			Craft A Letter
		</a>
	</div>

	<div
		class="bg-grade absolute inset-0 z-0 max-h-svh overflow-hidden bg-gradient-to-t from-red-500/10 to-gray-500/10"
	>
		<img src={LoveBg} alt="love-bg" class="size-full object-cover text-transparent opacity-75" />
	</div>
</main>

<style>
	.box-in {
		transition: transform, opacity;
		animation: grow-in 1s;
	}

	.bg-grade {
		animation: fade-in 0.3s;
	}

	.go-to {
		animation: shake 0s ease-in-out infinite;
	}

	@keyframes shake {
		0%,
		100% {
			transform: translateX(0);
		}
		25% {
			transform: translateX(-5px);
		}
		50% {
			transform: translateX(5px);
		}
		75% {
			transform: translateX(-5px);
		}
	}

	@keyframes grow-in {
		from {
			opacity: 0;
			transform: scale(0.8);
		}
		to {
			opacity: 1;
			transform: scale(1);
		}
	}

	@keyframes fade-in {
		from {
			opacity: 0;
		}
		to {
			opacity: 1;
		}
	}
</style>
