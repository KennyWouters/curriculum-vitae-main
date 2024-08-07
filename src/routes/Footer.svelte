<script>
	import { onMount } from 'svelte';


	let year;


	onMount(() => {
		year = new Date().getFullYear();
	});

	let socials = [
		{
			name: 'GitHub',
			icon: 'fa-brands fa-github',
			url: 'https://github.com/KennyWouters'
		},
		{
			name: 'Mail',
			icon: 'fa-solid fa-envelope',
			url: 'mailto:kenny.wouters2@gmail.com'
		},
		{
			name: 'Steam',
			icon: 'fa-brands fa-steam',
			url: 'https://steamcommunity.com/id/Kaynone/'
		},
		{
			name: 'LinkedIn',
			icon: 'fa-brands fa-linkedin',
			url: 'https://www.linkedin.com/in/kenny-wouters-408bb0279/'
		}
	]

</script>

<footer class="bg-transparent">
	<div class="mx-auto max-w-7xl px-6 py-12 md:flex md:items-center md:justify-between lg:px-8">
		<div class="flex justify-center space-x-6 md:order-2">


			{#each socials as social}
				<a href="{social.url}" class="text-gray-400 hover:text-gray-500">
					<span class="sr-only">{social.name}</span>
					<i class="{social.icon}"></i>
				</a>
				{/each}

		</div>
		<div class="mt-8 md:order-1 md:mt-0">
			<p class="text-center text-xs leading-5 text-gray-500">&copy; {year} Kenny Wouters. All rights reserved.</p>
<!--			<a href="https://data.typeracer.com/pit/profile?user=naeiur&ref=badge" target="_top"><img src="https://data.typeracer.com/misc/badge?user=naeiur" border="0" alt="TypeRacer.com scorecard for user naeiur"/></a>-->
		</div>
	</div>
</footer>
