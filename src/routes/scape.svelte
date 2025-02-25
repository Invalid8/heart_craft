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
	class="relative min-h-svh overflow-hidden bg-gradient-to-b from-pink-500/10 to-red-500/10 px-4 py-8"
>
	<!-- Decorative Elements for Desktop -->
	<div class="animate-float-slow absolute top-20 left-10 hidden opacity-20 lg:block">
		<svg
			width="120"
			height="120"
			viewBox="0 0 200 200"
			fill="none"
			xmlns="http://www.w3.org/2000/svg"
		>
			<path
				d="M100 170C80 150 30 110 30 65C30 35 55 20 75 20C90 20 100 35 100 35C100 35 110 20 125 20C145 20 170 35 170 65C170 110 120 150 100 170Z"
				fill="#FF4F92"
			/>
		</svg>
	</div>

	<div class="animate-float absolute top-40 right-16 hidden opacity-30 lg:block">
		<svg
			width="100"
			height="100"
			viewBox="0 0 200 200"
			fill="none"
			xmlns="http://www.w3.org/2000/svg"
		>
			<path
				d="M100 170C80 150 30 110 30 65C30 35 55 20 75 20C90 20 100 35 100 35C100 35 110 20 125 20C145 20 170 35 170 65C170 110 120 150 100 170Z"
				fill="#FF1A75"
			/>
		</svg>
	</div>

	<div class="animate-rotate absolute bottom-20 left-20 hidden opacity-25 lg:block">
		<svg
			width="80"
			height="80"
			viewBox="0 0 200 200"
			fill="none"
			xmlns="http://www.w3.org/2000/svg"
		>
			<path
				d="M100 170C80 150 30 110 30 65C30 35 55 20 75 20C90 20 100 35 100 35C100 35 110 20 125 20C145 20 170 35 170 65C170 110 120 150 100 170Z"
				fill="#FF85B3"
			/>
		</svg>
	</div>

	<div class="animate-float-slow absolute right-24 bottom-40 hidden opacity-20 lg:block">
		<svg
			width="90"
			height="90"
			viewBox="0 0 200 200"
			fill="none"
			xmlns="http://www.w3.org/2000/svg"
		>
			<path
				d="M100 170C80 150 30 110 30 65C30 35 55 20 75 20C90 20 100 35 100 35C100 35 110 20 125 20C145 20 170 35 170 65C170 110 120 150 100 170Z"
				fill="#FF4F92"
			/>
		</svg>
	</div>

	<!-- Stats on desktop left side -->
	<div
		class="absolute top-1/4 left-8 hidden flex-col items-center rounded-lg bg-white/80 p-4 shadow-lg backdrop-blur-sm lg:flex"
	>
		<div class="stat-item mb-4 text-center">
			<span class="block text-4xl font-bold text-red-500">15K+</span>
			<span class="text-gray-700">Love Notes Sent</span>
		</div>
		<div class="stat-item mb-4 text-center">
			<span class="block text-4xl font-bold text-red-500">98%</span>
			<span class="text-gray-700">Happy Recipients</span>
		</div>
		<div class="stat-item text-center">
			<span class="block text-4xl font-bold text-red-500">12</span>
			<span class="text-gray-700">Template Options</span>
		</div>
	</div>

	<!-- Live preview on desktop right side -->
	<div
		class="absolute top-1/4 right-8 hidden w-64 overflow-hidden rounded-lg bg-white/80 p-4 shadow-lg backdrop-blur-sm lg:block"
	>
		<h3 class="mb-2 text-center text-lg font-bold text-red-600">Live Preview</h3>
		<div
			class="preview-container relative flex h-48 items-center justify-center overflow-hidden rounded bg-gray-50"
		>
			<div class="z-10 px-3 text-center">
				<span class="font-medium">Your message will appear here</span>
				<p class="mt-2 text-sm text-gray-700">
					As you type, see your love note come to life in real-time
				</p>
			</div>
		</div>
		<div class="mt-4">
			<p class="text-sm text-gray-600 italic">
				"The perfect way to say 'I love you' in the digital age"
			</p>
			<p class="mt-1 text-right text-xs font-bold">- Love & Romance Magazine</p>
		</div>
	</div>

	<div class="relative z-10 mx-auto max-w-3xl">
		<!-- Header -->
		<header class="mb-8 text-center">
			<h1 class="mb-2 text-4xl font-bold text-red-600">HeartCraft</h1>
			<p class="text-lg text-gray-700">Create a heartfelt message for your special someone</p>
		</header>

		<!-- Main Content Box -->
		<div class="relative overflow-hidden rounded-xl bg-white p-6 shadow-xl md:p-8">
			<!-- Floating hearts decoration -->
			<div class="absolute top-0 right-0 -z-10 opacity-30">
				<svg
					width="200"
					height="200"
					viewBox="0 0 200 200"
					fill="none"
					xmlns="http://www.w3.org/2000/svg"
				>
					<path
						d="M100 170C80 150 30 110 30 65C30 35 55 20 75 20C90 20 100 35 100 35C100 35 110 20 125 20C145 20 170 35 170 65C170 110 120 150 100 170Z"
						fill="#FF4F92"
					/>
				</svg>
			</div>

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
					<div class="mb-6 grid grid-cols-1 gap-4 md:grid-cols-3">
						{#each templates as template}
							<button
								class={`template-card cursor-pointer rounded-lg p-4 transition-all ${selectedTemplate === template.id ? 'bg-pink-50 ring-4 ring-red-500' : 'border border-gray-200 hover:border-red-300'}`}
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

		<!-- Features/Benefits Section -->
		<div class="mt-12 grid grid-cols-1 gap-6 md:grid-cols-3">
			<div class="feature-card rounded-lg bg-white/80 p-4 text-center shadow-md backdrop-blur-sm">
				<div
					class="mx-auto mb-4 flex h-16 w-16 items-center justify-center rounded-full bg-red-100"
				>
					<svg
						xmlns="http://www.w3.org/2000/svg"
						class="h-8 w-8 text-red-500"
						fill="none"
						viewBox="0 0 24 24"
						stroke="currentColor"
					>
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"
						/>
					</svg>
				</div>
				<h3 class="mb-2 text-xl font-bold text-gray-800">Express Yourself</h3>
				<p class="text-gray-600">
					Share your deepest feelings with beautiful animations and heartfelt messages
				</p>
			</div>
			<div class="feature-card rounded-lg bg-white/80 p-4 text-center shadow-md backdrop-blur-sm">
				<div
					class="mx-auto mb-4 flex h-16 w-16 items-center justify-center rounded-full bg-red-100"
				>
					<svg
						xmlns="http://www.w3.org/2000/svg"
						class="h-8 w-8 text-red-500"
						fill="none"
						viewBox="0 0 24 24"
						stroke="currentColor"
					>
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z"
						/>
					</svg>
				</div>
				<h3 class="mb-2 text-xl font-bold text-gray-800">Surprise Them</h3>
				<p class="text-gray-600">
					Send your love note instantly or schedule it for a special moment
				</p>
			</div>
			<div class="feature-card rounded-lg bg-white/80 p-4 text-center shadow-md backdrop-blur-sm">
				<div
					class="mx-auto mb-4 flex h-16 w-16 items-center justify-center rounded-full bg-red-100"
				>
					<svg
						xmlns="http://www.w3.org/2000/svg"
						class="h-8 w-8 text-red-500"
						fill="none"
						viewBox="0 0 24 24"
						stroke="currentColor"
					>
						<path
							stroke-linecap="round"
							stroke-linejoin="round"
							stroke-width="2"
							d="M5 3v4M3 5h4M6 17v4m-2-2h4m5-16l2.286 6.857L21 12l-5.714 2.143L13 21l-2.286-6.857L5 12l5.714-2.143L13 3z"
						/>
					</svg>
				</div>
				<h3 class="mb-2 text-xl font-bold text-gray-800">Make Memories</h3>
				<p class="text-gray-600">
					Create lasting impressions that your loved one will cherish forever
				</p>
			</div>
		</div>

		<!-- Testimonials Section - Added for desktop fullness -->
		<div class="mt-12 rounded-xl bg-white/80 p-6 shadow-lg backdrop-blur-sm">
			<h2 class="mb-6 text-center text-2xl font-bold text-gray-800">Love Stories from Our Users</h2>
			<div class="grid grid-cols-1 gap-6 md:grid-cols-3">
				{#each testimonials as testimonial, i}
					<div class="testimonial relative rounded-lg bg-pink-50 p-4">
						<div class="absolute -top-4 -right-2 text-6xl text-pink-500 opacity-20">"</div>
						<div class="mb-3 flex items-center">
							<div class="mr-3 h-12 w-12 overflow-hidden rounded-full bg-gray-300">
								<img
									src={testimonial.avatar}
									alt={testimonial.name}
									class="h-full w-full object-cover"
								/>
							</div>
							<div>
								<h4 class="font-bold">{testimonial.name}</h4>
								<div class="flex text-yellow-400">
									<svg
										xmlns="http://www.w3.org/2000/svg"
										class="h-4 w-4"
										viewBox="0 0 20 20"
										fill="currentColor"
									>
										<path
											d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"
										/>
									</svg>
									<svg
										xmlns="http://www.w3.org/2000/svg"
										class="h-4 w-4"
										viewBox="0 0 20 20"
										fill="currentColor"
									>
										<path
											d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"
										/>
									</svg>
									<svg
										xmlns="http://www.w3.org/2000/svg"
										class="h-4 w-4"
										viewBox="0 0 20 20"
										fill="currentColor"
									>
										<path
											d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"
										/>
									</svg>
									<svg
										xmlns="http://www.w3.org/2000/svg"
										class="h-4 w-4"
										viewBox="0 0 20 20"
										fill="currentColor"
									>
										<path
											d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"
										/>
									</svg>
									<svg
										xmlns="http://www.w3.org/2000/svg"
										class="h-4 w-4"
										viewBox="0 0 20 20"
										fill="currentColor"
									>
										<path
											d="M9.049 2.927c.3-.921 1.603-.921 1.902 0l1.07 3.292a1 1 0 00.95.69h3.462c.969 0 1.371 1.24.588 1.81l-2.8 2.034a1 1 0 00-.364 1.118l1.07 3.292c.3.921-.755 1.688-1.54 1.118l-2.8-2.034a1 1 0 00-1.175 0l-2.8 2.034c-.784.57-1.838-.197-1.539-1.118l1.07-3.292a1 1 0 00-.364-1.118L2.98 8.72c-.783-.57-.38-1.81.588-1.81h3.461a1 1 0 00.951-.69l1.07-3.292z"
										/>
									</svg>
								</div>
							</div>
						</div>
						<p class="text-gray-700">{testimonial.text}</p>
					</div>
				{/each}
			</div>
		</div>

		<!-- Call to Action -->
		<div class="mt-12 text-center">
			<p class="mb-4 text-xl text-gray-800">Ready to send a little love into someone's day?</p>
			<button
				class="rounded-full bg-red-600 px-8 py-3 text-lg font-bold text-white shadow-lg transition-colors hover:bg-red-700"
				onclick={() => (currentStep = 1)}
			>
				Create Your Love Note Now
			</button>
		</div>
	</div>

	<!-- Preview Modal -->
	{#if isPreviewOpen}
		<div
			class="fixed inset-0 z-50 flex items-center justify-center bg-black/70 p-4 backdrop-blur-sm"
		>
			<div
				bind:this={previewElement}
				class="scale-in-center relative w-full max-w-md overflow-hidden rounded-lg bg-[#f9c0dc] shadow-2xl"
			>
				<!-- Template Preview based on selectedTemplate -->
				<div class="p-6">
					<div class="mb-4 text-center">
						<h3 class="text-2xl font-bold">Will You Be Mine?</h3>
					</div>
					<div class="space-y-3 text-center">
						<Typewriter wordInterval={50000} mode={'cascade'} cursor={false}></Typewriter>
						<p>
							{messageText ||
								"From the moment our paths crossed, I've felt a connection that words struggle to capture. Your smile brightens even my darkest days."}
						</p>
						<p class="mt-4 font-medium">
							With love,<br />
							{senderName || 'Your Secret Admirer'}
						</p>
					</div>
					<div class="mt-6 flex justify-center">
						<button
							class="rounded-full bg-red-500 px-6 py-2 font-bold text-white transition-colors hover:bg-red-600"
							onclick={togglePreview}
						>
							Close Preview
						</button>
					</div>
				</div>
				<!-- Decorative background -->
				<div class="absolute inset-0 -z-10 opacity-20">
					<svg class="h-full w-full" viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
						<path
							fill="#FF4F92"
							d="M43.2,-57.2C55.9,-49.7,66.2,-37.2,71.1,-22.5C76,-7.9,75.5,8.8,70.1,23.5C64.7,38.1,54.3,50.7,41.3,58.3C28.3,65.9,12.7,68.4,-1.9,71C-16.5,73.5,-33,76.1,-45.6,69.1C-58.1,62.1,-66.7,45.6,-71.5,28.8C-76.3,12,-77.3,-5.1,-73.2,-20.7C-69.1,-36.3,-59.8,-50.3,-46.8,-57.7C-33.8,-65.1,-16.9,-65.9,-0.7,-65C15.4,-64.1,30.5,-64.7,43.2,-57.2Z"
							transform="translate(100 100)"
						/>
					</svg>
				</div>
			</div>
		</div>
	{/if}
</main>

<style>
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

	@keyframes float {
		0% {
			transform: translateY(0);
		}
		50% {
			transform: translateY(-10px);
		}
		100% {
			transform: translateY(0);
		}
	}

	@keyframes float-slow {
		0% {
			transform: translateY(0);
		}
		50% {
			transform: translateY(-15px);
		}
		100% {
			transform: translateY(0);
		}
	}

	@keyframes rotate {
		0% {
			transform: rotate(0deg);
		}
		100% {
			transform: rotate(360deg);
		}
	}

	.animate-float {
		animation: float 6s ease-in-out infinite;
	}

	.animate-float-slow {
		animation: float-slow 6s ease-in-out infinite;
	}
</style>
