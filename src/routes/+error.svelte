<script>
	import { page } from '$app/stores';
	import { cn } from '$lib';
	import { canGoBack, goBack, isExempted } from '$lib/common';
</script>

<main
	class="main relative grid size-full h-svh place-content-center place-items-center bg-red-500/10"
>
	<div
		class={cn(
			'error_page grid h-full w-full place-content-center place-items-center p-4 md:p-6',
			isExempted($page.url.pathname) && 'min-h-svh'
		)}
	>
		<div class="info flex flex-col items-center gap-2 text-center">
			<div class="box-w-48">
				<video
					class="vid size-full bg-inherit object-contain object-center mix-blend-multiply"
					playsInline
					autoPlay={true}
					muted={false}
					loop={true}
					controls={false}
				>
					<source src="/bear.webm" type="video/webm" />
				</video>
			</div>
			<h1 class="sp-h mb-0 pb-0 text-4xl font-bold">
				{$page?.error?.message || 'An Unexpected Error Occurred'}
			</h1>
			<div class="btn-container mx-auto">
				<button
					onclick={goBack}
					class="flex items-center justify-center gap-2 rounded-md bg-black px-6 py-1.5 font-medium text-white"
				>
					{canGoBack() ? 'Back' : 'Home'}
				</button>
			</div>
		</div>
	</div>
</main>
