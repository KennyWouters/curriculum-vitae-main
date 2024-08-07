<script>

	import {goto} from '$app/navigation';
	import {onMount} from 'svelte';
	import {navbarStore } from './store.js';

	let activeIndex;

	navbarStore.subscribe(value => {
		activeIndex = value.index;
	});

	let links = [
		{ name: 'About Me', url: '/', visible:false},
		{ name: 'My curriculum', url: '/curriculum', visible:false},
		{ name: 'My Projects', url: '/projects', visible:false},
		{ name: 'Contacting Me', url: '/contact', visible:false}
	]

	const navigateAndSetIndex = (index, url) => {
		open = false
		// activeIndex = index
		goto(url)
	}

	let open = false;
	
	const openMenu = () => {
		open = !open
	}

	onMount(() => {
		links.forEach((link, index) => {
			setTimeout(() => {
				links[index] = { ...link, visible: true };
			}, 200 * (index + 1));
		});
	});

</script>



<nav class="bg-gray-800">
	<div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
		<div class="flex h-16 items-center justify-between">
			<div class="flex items-center">
				<div class="hidden sm:ml-6 sm:block">
					<div class="flex space-x-4">

						{#each links as link, index}
							{#if activeIndex === index}
								<button on:click={() => {navigateAndSetIndex(index, link.url)}} class="{link.visible ? '' : 'hidden'} rounded-md bg-gray-900 px-3 py-2 text-sm font-medium text-white animate-quickerFadeIn">{link.name}</button>
								{:else}
								<button on:click={() => {navigateAndSetIndex(index, link.url)}} class="{link.visible ? '' : 'hidden'} rounded-md px-3 py-2 text-sm font-medium text-gray-300 hover:bg-gray-700 hover:text-white animate-quickerFadeIn">{link.name}</button>
								{/if}
						{/each}
					</div>
				</div>
			</div>
			<div class="-mr-2 flex sm:hidden">
				<!-- Mobile menu button -->
				<button on:click={() => {openMenu()}} type="button" class="relative inline-flex items-center justify-center rounded-md p-2 text-gray-400 hover:bg-gray-700 hover:text-white focus:outline-none focus:ring-2 focus:ring-inset focus:ring-white" aria-controls="mobile-menu" aria-expanded="false">
					<span class="absolute -inset-0.5"></span>
					<span class="sr-only">Open main menu</span>
					<!--
						Icon when menu is closed.

						Menu open: "hidden", Menu closed: "block"
					-->
					<svg class="{open ? 'hidden' : 'block'} h-6 w-6" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" aria-hidden="true">
						<path stroke-linecap="round" stroke-linejoin="round" d="M3.75 6.75h16.5M3.75 12h16.5m-16.5 5.25h16.5" />
					</svg>
					<!--
						Icon when menu is open.

						Menu open: "block", Menu closed: "hidden"
					-->
					<svg class="{open ? 'block' : 'hidden'} h-6 w-6" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" aria-hidden="true">
						<path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
					</svg>
				</button>
			</div>
		</div>
	</div>

	<!-- Mobile menu, show/hide based on menu state. -->
	<div class="sm:hidden {open ? '' : 'hidden'}" id="mobile-menu">
		<div class="space-y-1 px-2 pb-3 pt-2">

			{#each links as link, index}
				{#if activeIndex === index}
					<button on:click={() => {navigateAndSetIndex(index, link.url)}} class="{link.visible ? '' : 'hidden'} block rounded-md bg-gray-900 px-3 py-2 text-base font-medium text-white">{link.name}</button>
				{:else}
					<button on:click={() => {navigateAndSetIndex(index, link.url)}} class="{link.visible ? '' : 'hidden'} block rounded-md px-3 py-2 text-base font-medium text-gray-300 hover:bg-gray-700 hover:text-white">{link.name}</button>
				{/if}
			{/each}

		</div>
	</div>
</nav>
