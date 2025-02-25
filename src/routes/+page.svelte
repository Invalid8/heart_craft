<script lang="ts">
	import { onMount } from 'svelte';
	import Typewriter from 'svelte-typewriter';

	let selectedTemplate: string = $state('teddy');
	let messageText: string = $state('');
	let senderName: string = $state('');
	let recipientEmail: string = $state('');
	let isPreviewOpen: boolean = $state(false);
	let previewElement: HTMLDivElement | null = $state(null);
	let currentStep: number = $state(1);
	let templates = [
		{ id: 'teddy', name: 'Teddy Bear', image: '/templates/teddy.svg' },
		{ id: 'flowers', name: 'Flowers', image: '/templates/flower.svg' },
		{ id: 'chocolate', name: 'Chocolates', image: '/templates/chocolate.svg' }
	];

	// Testimonials data
	let testimonials = [
		{
			name: 'Jessica R.',
			avatar: '/avatars/avatar-g-1.svg',
			text: 'My boyfriend was so surprised when he got my love note! The animation made it extra special.'
		},
		{
			name: 'Michael T.',
			avatar: '/avatars/avatar-b-2.svg',
			text: 'I used this to propose to my girlfriend and she loved it! Best decision ever.'
		},
		{
			name: 'Samantha K.',
			avatar: '/avatars/avatar-b-1.svg',
			text: 'Such a creative way to express my feelings on our anniversary. Worth every penny!'
		}
	];

	function nextStep(): void {
		if (currentStep < 3) {
			currentStep++;
		}
	}

	function prevStep(): void {
		if (currentStep > 1) {
			currentStep--;
		}
	}

	function togglePreview(): void {
		isPreviewOpen = !isPreviewOpen;
	}

	function handleClickOutside(event: MouseEvent): void {
		const target = event.target as Node;
		if (isPreviewOpen && previewElement && !previewElement.contains(target)) {
			isPreviewOpen = false;
		}
	}

	function selectTemplate(id: string): void {
		selectedTemplate = id;
	}

	function sendLetter(): void {
		// Logic to save and send the letter would go here
		alert('Your love letter has been sent! ❤️');
		// Reset form
		messageText = '';
		senderName = '';
		recipientEmail = '';
		currentStep = 1;
	}

	onMount(() => {
		document.addEventListener('click', handleClickOutside);

		// Cleanup
		return () => {
			document.removeEventListener('click', handleClickOutside);
		};
	});
</script>

<main
	class="main relative grid size-full h-svh place-content-center place-items-center bg-red-500/10 p-4 sm:p-6"
>
	<div class="relative z-10 overflow-hidden rounded-xl bg-white/98 p-6 shadow-xl md:p-8 w-full md:w-3xl sm:w-xl max-w-3xl">
		<!-- Step indicator -->
		<div class="mb-8 flex items-center justify-center">
			<div
				class={`flex h-8 w-8 items-center justify-center rounded-full ${currentStep >= 1 ? 'bg-red-500 text-white' : 'bg-gray-200'}`}
			>
				1
			</div>
			<div class={`h-1 w-12 ${currentStep >= 2 ? 'bg-red-500' : 'bg-gray-200'}`}></div>
			<div
				class={`flex h-8 w-8 items-center justify-center rounded-full ${currentStep >= 2 ? 'bg-red-500 text-white' : 'bg-gray-200'}`}
			>
				2
			</div>
			<div class={`h-1 w-12 ${currentStep >= 3 ? 'bg-red-500' : 'bg-gray-200'}`}></div>
			<div
				class={`flex h-8 w-8 items-center justify-center rounded-full ${currentStep >= 3 ? 'bg-red-500 text-white' : 'bg-gray-200'}`}
			>
				3
			</div>
		</div>

		<!-- Step 1: Choose Template -->
		{#if currentStep === 1}
			<div class="step-content">
				<h2 class="mb-4 text-center text-2xl font-bold text-gray-800">Choose Your Template</h2>
				<div class="mb-6 md:grid gap-4 md:grid-cols-3 flex p-2 pb-4 max-w-[calc(100vw_-_6rem)] overflow-x-auto">
					{#each templates as template}
						<button
							class={`min-w-[200px] template-card cursor-pointer rounded-lg p-4 transition-all ${selectedTemplate === template.id ? 'bg-pink-50 ring-4 ring-red-500' : 'border border-gray-200 hover:border-red-300'}`}
							onclick={() => selectTemplate(template.id)}
						>
							<img
								src={template.image}
								alt={template.name}
								class="mb-2 h-32 w-full rounded-md object-contain"
							/>
							<p class="text-center font-medium">{template.name}</p>
						</button>
					{/each}
				</div>
				<div class="mt-4 text-center">
					<button
						class="rounded-full bg-red-500 px-8 py-2 font-bold text-white shadow-md transition-colors hover:bg-red-600"
						onclick={nextStep}
					>
						Continue →
					</button>
				</div>
			</div>
		{/if}

		<!-- Step 2: Write Message -->
		{#if currentStep === 2}
			<div class="step-content">
				<h2 class="mb-4 text-center text-2xl font-bold text-gray-800">Write Your Message</h2>
				<div class="mb-6">
					<label for="message" class="mb-2 block text-gray-700">Your Heartfelt Message:</label>
					<textarea
						id="message"
						bind:value={messageText}
						class="h-40 w-full rounded-lg border border-gray-300 p-3 focus:border-red-300 focus:ring-2 focus:ring-red-300"
						placeholder="From the moment our paths crossed, I've felt a connection that words struggle to capture..."
					></textarea>
				</div>
				<div class="mb-6">
					<label for="sender" class="mb-2 block text-gray-700">Your Name:</label>
					<input
						type="text"
						id="sender"
						bind:value={senderName}
						class="w-full rounded-lg border border-gray-300 p-3 focus:border-red-300 focus:ring-2 focus:ring-red-300"
						placeholder="Your name"
					/>
				</div>
				<div class="mt-6 flex justify-between">
					<button
						class="rounded-full bg-gray-300 px-6 py-2 font-bold text-gray-800 transition-colors hover:bg-gray-400"
						onclick={prevStep}
					>
						← Back
					</button>
					<button
						class="rounded-full bg-red-500 px-8 py-2 font-bold text-white shadow-md transition-colors hover:bg-red-600"
						onclick={nextStep}
						disabled={!messageText || !senderName}
					>
						Continue →
					</button>
				</div>
			</div>
		{/if}

		<!-- Step 3: Recipient & Preview -->
		{#if currentStep === 3}
			<div class="step-content">
				<h2 class="mb-4 text-center text-2xl font-bold text-gray-800">Send Your Love</h2>
				<div class="mb-6">
					<label for="recipient" class="mb-2 block text-gray-700">Recipient's Email:</label>
					<input
						type="email"
						id="recipient"
						bind:value={recipientEmail}
						class="w-full rounded-lg border border-gray-300 p-3 focus:border-red-300 focus:ring-2 focus:ring-red-300"
						placeholder="their.email@example.com"
					/>
				</div>
				<div class="mb-6 text-center">
					<button
						class="mb-4 rounded-full bg-pink-400 px-6 py-2 font-bold text-white transition-colors hover:bg-pink-500"
						onclick={togglePreview}
					>
						Preview Your Message
					</button>
					<p class="text-sm text-gray-600">
						See how your love note will appear to your special someone
					</p>
				</div>
				<div class="mt-6 flex justify-between">
					<button
						class="rounded-full bg-gray-300 px-6 py-2 font-bold text-gray-800 transition-colors hover:bg-gray-400"
						onclick={prevStep}
					>
						← Back
					</button>
					<button
						class="animate-pulse rounded-full bg-red-500 px-8 py-2 font-bold text-white shadow-md transition-colors hover:bg-red-600"
						onclick={sendLetter}
						disabled={!recipientEmail}
					>
						Send With Love ❤️
					</button>
				</div>
			</div>
		{/if}
	</div>
</main>

<style>
	main::after {
		content: '';
		position: absolute;
		opacity: 0.5;
		width: 100%;
		height: 100%;
		background-image: url('/love-bg-black.svg');
		background-size: cover;
		background-position: center;
		background-repeat: no-repeat;
	}
</style>
