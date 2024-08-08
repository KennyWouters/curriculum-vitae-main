<script>
	import { onMount, beforeUpdate, tick } from 'svelte';
	import {navbarStore} from '../store.js';
	import {goto} from '$app/navigation';

	let experiences = [

		{
			date: 'Apr 2024 - May 2024',
			title: 'Dotnet Developer Intern',
			company: "Radyo",
			location: "Namur",
			icon:"fa-solid fa-code",
			responsibilities: [
				" Software engineering on clean architecture DDD API",
				" Working with GraphQL / MassTransit / MediaTR / EFCore / SignalR",
				" Develop and maintain the integration/unit testing of the app",
				" Working with Docker / Kubernetes / Azure DevOps"
			]
		},

		{
			date: 'Nov 2021 - Dec 2022',
			title: 'Co-Founder & Manager',
			company: "L'aile ou la cuisse",
			location: "Waremme",
			icon:"fa-solid fa-drumstick-bite",
			responsibilities: [
				"Managing , training, and supervising staff",
				"Developing corporate identity (visuals , social media accounts ) and managing marketing campaigns"
			]
		},
		{
			date: 'Jul 2018 - Nov 2021',
			title: 'Qualified Agent (Security)',
			company: "G4S, / Fact Security",
			location: "Brussels",
			icon:"fa-solid fa-eye",
			responsibilities: [
				"Conflict resolution and prevention"
			]
		},

		{
			date: 'Jun 2015 - Sep 2017',
			title: 'Graphic Designer / Assistant to the Manager',
			company: "Slogans SPRL",
			location: "Floreffe",
			icon:"fa-solid fa-paint-roller",
			responsibilities: [
				"Managing client relationships"
			]
		}
	]

	let studies = [

		{
			date: 'Aug 2024 - Present',
			title: '.NET 8 Microservices, API, Clean Architecture',
			company: "Udemy",
			location: "Remote",
			icon:"fa-solid fa-code",
			responsibilities: [
				"Building Clean Architecture Micro-services/API following DDD principles",
				"Mastering libraries / frameworks such as Carter, MediatR, Mapster, Masstransit, Fluent Validation, EFCore, Refit, Redis, gRPC, RabbitMQ, Yarp API Gateway",
				"Specializing in Docker, Docker Compose for containerization"
			]
		},
		{
			date: 'Jun 2018 - Mar 2019 (Interrupted)',
			title: 'Full Stack Web Developer',
			company: "Technifutur",
			location: "Seraing",
			icon:"fa-solid fa-code",
			responsibilities: [
				"Developing web applications with Dotnet",
				"Working with APIs",
				"Working with databases",
				"Developing web applications with Angular",
				"Agile methodology",
				"Scrum / Kanban",
				"Azure DevOps"
			]
		},

		{
			date: 'Jul 2023 - Actual',
			title: 'Python Developer',
			company: "The App Brewery",
			location: "London",
			icon:"fa-brands fa-python",
			responsibilities: [
				"Basic programming concepts",
				"Working with APIs",
				"Working with databases",
				"Developing web applications with Django / Flask"
			]
		},

		{
			date: 'Jan 2010 - Feb 2010',
			title: 'Cisco ITEssentials',
			company: "Technifutur",
			location: "Ciney",
			icon:"fa-solid fa-eye",
			responsibilities: [
				"",

			]
		},

		{
			date: 'Sep 2007 - Jun 2008',
			title: 'Photography',
			company: "Inraci",
			location: "Brussels",
			icon:"fa-solid fa-print",
			responsibilities: [
				"Technical drawing",
				"Graphic design",
				"Computer graphics"
			]
		}

	]

	let activeTab = 0


	let tabs = [
		{
			title: 'Career',
			icon: 'fa-solid fa-briefcase',
			content: experiences
		},
		{
			title: 'Studies',
			icon: 'fa-solid fa-graduation-cap',
			content: studies
		},
		{
			title: 'Typing',
			icon: 'fa-regular fa-keyboard',
			content: ''
		}
	]

	const changeActiveTab = (index) => {
		activeTab = index
	}

	const navigateToProjects = () => {
		goto('/projects');
	}

	onMount(() => {
		navbarStore.update(n => {
			return {...n, index: 1};
		});

		// Update windowWidth whenever the window is resized

		if (typeof window !== 'undefined'){
			windowWidth = window.innerWidth;

			window.addEventListener('resize', () => {
				windowWidth = window.innerWidth;
			});
		}

		let breathInterval = setInterval(async () => {
			buttonBreathe = true;
			await tick();
			await new Promise(r => setTimeout(r, 1000));
			buttonBreathe = false;
			await tick();
			await new Promise(r => setTimeout(r, 1000));
		}, 2000); // 2000ms: main interval that triggers the button "breath" effect every 2 seconds

	});

	let buttonBreathe = false;

	beforeUpdate(() => {
		if (typeof window !== 'undefined'){
			windowWidth = window.innerWidth;
		}
	});

	let x;
	let y;
	let box;
	let boxWidth = 384;
	let windowWidth;

	$: if (typeof window !== 'undefined') {
		windowWidth = window.innerWidth;
	}
	
	const handleMouseMove = (event) => {
		x = event.clientX;
		y = event.clientY;

		// Adjust x if the box is too close to the right edge
		if (x + boxWidth > windowWidth) {
			x = windowWidth - boxWidth;
		}
	}

	let description = [];

	let activeElementIndex = 0;

	const handleMouseOverElement = (index) => {
		activeElementIndex = index;
		displayDescription = true;
		let activeDic = activeTab === 0 ? experiences : studies;
		description = activeDic[index].responsibilities;
	}

	//method that checks if an index in parameter is the same as the activeElementIndex, and if so, it sets displayDescription to false
	const handleClickOnElement = (index) => {
		if (index === activeElementIndex) {
			displayDescription = false;
		}
	}

	const handleMouseOutElement = () => {
		displayDescription = false;
	}

	let displayDescription = false

	function handleHover(node, params) {
		// Handle mouse events
		node.addEventListener('mouseover', () => handleMouseOverElement(params.index));
		node.addEventListener('mouseout', handleMouseOutElement);
		node.addEventListener('click', () => handleClickOnElement(params.index));

		// Handle touch events
		node.addEventListener('touchstart', (event) => {
			handleMouseMove(event.touches[0])
			handleMouseOverElement(params.index)
		});
		node.addEventListener('touchend', handleMouseOutElement);
		node.addEventListener('touchcancel', handleMouseOutElement);

		return {
			destroy() {
				// Clean up event listeners
				node.removeEventListener('mouseover', () => handleMouseOverElement(params.index));
				node.removeEventListener('mouseout', handleMouseOutElement);
				node.removeEventListener('click', () => handleClickOnElement(params.index));
				node.removeEventListener('touchstart', () => handleMouseOverElement(params.index));
				node.removeEventListener('touchend', handleMouseOutElement);
				node.removeEventListener('touchcancel', handleMouseOutElement);
			}
		};
	}


</script>



<div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8">
	<!-- We've used 3xl here, but feel free to try other max-widths based on your needs -->
	<div class="mx-auto max-w-3xl">




<!--						SELECTOR-->
		<div class="sm:mt-32 animate-quickerFadeIn">
			<div class="sm:flex sm:items-baseline">
				<div class="mt-4 sm:ml-10 sm:mt-0">
					<nav class="-mb-px flex space-x-8 justify-center">

						{#each tabs as tab, index}
							{#if (index === activeTab)}
								<button on:click={() => {changeActiveTab(index)}} class="border-indigo-500 text-indigo-600 whitespace-nowrap border-b-2 px-1 pb-4 text-sm font-medium" aria-current="page"><i class="{tab.icon}"></i> {tab.title}</button>
							{:else}
								<button on:click={() => {changeActiveTab(index)}} class="border-transparent text-gray-500 hover:border-gray-300 hover:text-gray-700 whitespace-nowrap border-b-2 px-1 pb-4 text-sm font-medium"><i class="{tab.icon}"></i> {tab.title}
								</button>
							{/if}
						{/each}

					</nav>
				</div>
			</div>
		</div>



<!--						EXPERIENCES CONTENT-->
		<ul role="list" class="{activeTab == 0 ? '' : 'hidden'} bg-white divide-y divide-gray-200 rounded-2xl border-2 p-5 animate-quickerFadeIn">


			{#each experiences as exp, index}
				<li use:handleHover={{ index }} class="flex justify-between gap-x-6 py-5 select-none" on:mousemove={handleMouseMove}>
					<div class="flex min-w-0 gap-x-4">
							<i class="{exp.icon}"></i>
						<div class="min-w-0 flex-auto">
							<p class="text-sm font-semibold leading-6 text-gray-900">{exp.company}</p>
							<p class="mt-1 truncate text-xs leading-5 text-gray-500">{exp.title}</p>
						</div>
					</div>
					<div class="hidden shrink-0 sm:flex sm:flex-col sm:items-end">
						<p class="text-sm leading-6 text-gray-900">{exp.date}</p>
						<p class="mt-1 text-xs leading-5 text-gray-500">{exp.location}</p>
					</div>
				</li>
				{/each}


		</ul>


<!--STUDIES CONTENT-->
		<ul role="list" class="{activeTab == 1 ? '' : 'hidden'} bg-white divide-y divide-gray-200 rounded-2xl border-2 p-5 animate-quickerFadeIn">


			{#each studies as exp, index}
				<li use:handleHover={{ index }} class="flex justify-between gap-x-6 py-5 select-none" on:mousemove={handleMouseMove}>
					<div class="flex min-w-0 gap-x-4">
						<i class="{exp.icon}"></i>
						<div class="min-w-0 flex-auto">
							<p class="text-sm font-semibold leading-6 text-gray-900">{exp.title} <i class="fa-solid fa-graduation-cap"></i></p>
							<p class="mt-1 truncate text-xs leading-5 text-gray-500">{exp.company}</p>
						</div>
					</div>
					<div class="hidden shrink-0 sm:flex sm:flex-col sm:items-end">
						<p class="text-sm leading-6 text-gray-900">{exp.date}</p>
						<p class="mt-1 text-xs leading-5 text-gray-500">{exp.location}</p>
					</div>
				</li>
			{/each}


		</ul>

<!--		TYPING CONTENT-->
		<a class="{activeTab == 2 ? '' : 'hidden'} mt-12 sm:mt-2 animate-quickerFadeIn flex justify-center items-center" href="https://data.typeracer.com/pit/profile?user=naeiur&ref=badge" target="_top">
			<img class="w-3/5 sm:w-2/5 md:w-1/3 lg:w-2/5 xl:w-1.5/5" src="https://data.typeracer.com/misc/badge?user=naeiur" border="0" alt="TypeRacer.com scorecard for user naeiur"/>
		</a>
	</div>

	<div class="flex justify-center items-center mt-12">
		<button on:click={() => {navigateToProjects()}} type="button" class="{buttonBreathe ? 'normal-button' : 'big-button'} rounded-full bg-indigo-600 px-4 py-2.5 text-sm font-semibold text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600">
			Take a look at my projects
		</button>
	</div>

</div>



<div style="position: absolute; top: {y}px; left: {x}px; pointer-events: none; width: {boxWidth}px" class="border-2 border-black pl-4 rounded-md bg-green-300 p-4 {displayDescription ? '' : 'hidden'}">
	<div class="flex">
		<div class="ml-3">
			{#each description as desc}
				<p class="text-sm text-black mb-2">• {desc}</p>
			{/each}
		</div>
		<div class="ml-auto pl-3">
			<div class="-mx-1.5 -my-1.5">
			</div>
		</div>
	</div>
</div>

