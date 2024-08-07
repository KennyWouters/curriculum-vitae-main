<script>
	import {navbarStore} from '../store.js';
	import {onMount} from 'svelte';
	import pic1_1 from '$lib/images/project_1/1.png';
	import pic1_2 from '$lib/images/project_1/2.png';
	import pic1_3 from '$lib/images/project_1/3.png';
	import pic1_4 from '$lib/images/project_1/4.png';
	import pic2_1 from '$lib/images/project_2/1.png';
	import pic2_2 from '$lib/images/project_2/2.png';
	import pic2_3 from '$lib/images/project_2/3.png';
	import pic3_1 from '$lib/images/project_3/1.png';
	import pic3_2 from '$lib/images/project_3/2.png';
	import pic3_3 from '$lib/images/project_3/3.png';
	import pic3_4 from '$lib/images/project_3/4.png';
	import pic3_5 from '$lib/images/project_3/5.png';

	onMount(() => {
		navbarStore.update(n => {
			return {...n, index: 2};
		});


		for (let i = 0; i < projects.length; i++) {
			for (let j = 0; j < projects[i].tags.length; j++) {
				if (!tagList.includes(projects[i].tags[j])) {
					tagList.push(projects[i].tags[j]);
				}
			}
		}

		for (let i = 0; i < tagList.length; i++) {
			tagStates = [...tagStates, {tag: tagList[i], selected: true}];
		}

		for (let i = 0; i < tagList.length; i++) {
			selectedTags = [...selectedTags, tagList[i]];
		}
	});

	let projects = [
		{
			name: 'Greenspheres.org',
			description: "Even though front end and design can't be said to be my cup of tea, I still had a lot of fun creating this website for a french client in the CSR domain.\n\nThe website is a simple static one that I created using SvelteKit and TailwindCSS",
			link: 'https://www.greenspheres.org/',
			captions: [
				{
					picture: pic2_1,
					description: ''
				},
				{
					picture: pic2_2,
					description: ''
				},
				{
					picture: pic2_3,
					description: ''
				}
			],
			tags: ['Front-End', 'SvelteKit', 'TailwindCSS', 'Website', 'Job']
		},
		{
			name: "TypeDungeon",
			description: 'TypeDungeon is a little prototype game I created in the context of my C# initiation course\nI took it way more seriously than I was asked to and had way too much fun with it.\n\nI went as far as creating a custom rudimentary map creation system',
			link: 'https://github.com/SamGaban/Type-Dungeon?tab=readme-ov-file',
			captions: [
				{
					picture: pic1_1,
					description: 'The project helped me get acquainted with C# and essential parts of the language such as inheritance, polymorphism, and interfaces'
				},
				{
					picture: pic1_2,
					description: 'Taking it way further than the course required me to, I implemented a simple inventory system and a basic combat system, but also room where enemies could spawn following a difficulty per room'
				},
				{
					picture: pic1_3,
					description: 'Found out that implementing multiple choice that you could select with arrow keys was way more enjoyable than typing the answer, so I implemented that as well'
				},
				{
					picture: pic1_4,
					description: 'The funniest part of it all, is that even as it was my first game project, and inexperienced as I was, I still managed to make the combat system something that I fell in love with, a typing game where you had to type the sentences displaying randomly on the screen to defeat the enemies (fun fact: it was then that I was for the first time confronted to asynchronous programming)'
				}
			],
			tags: ['C#', 'Game', 'Typing']
		},
		{
			name: "Typing Town",
			description: 'Typing Town is a typing game I created using Unity and C#.\n\nThe game puts you in the shoes of a mage that has to type in sets of words to cast spells and defeat enemies.\n\nThe game was created in the context of learning game development with real engines and I had a blast putting it together\n\nI eventually had to let it go as a lot of bad practices were used in the code and I didn\'t have the time to refactor it',
			link: 'https://github.com/SamGaban/RoleTypingGame',
			captions: [
				{
					picture: pic3_1,
					description: 'An animated main menu, unity UI building is a real pain, but it was a fun challenge to tackle'
				},
				{
					picture: pic3_2,
					description: 'By successfully completing the levels, you could unlock new spells in your hub town and decorate it with the money you earned from the levels'
				},
				{
					picture: pic3_3,
					description: 'The level selection screen allowed you to choose a difficulty and a level to play, the difficulty would affect the number of enemies and the number of words you had to type to defeat them'
				},
				{
					picture: pic3_4,
					description: 'You could also switch between spell-casting and spear, just so if you were outnumbered, you could still have a chance to defend yourself'
				},
				{
					picture: pic3_5,
					description: 'Also, the DLL I created to handle the typing was pretty complete and allowed for things like feedback on your speed and accuracy, and also a way to add new words to the game easily'
				}
			],
			tags: ['C#', 'Game', 'Typing', 'Unity']
		}
	]

	let tagList = [];
	let tagStates = [];
	let selectedProject = projects[0];

	let selectedTags = [];

	const updateSelectedTags = (index) => {

		if (selectedTags.length == 1 && tagStates[index].selected) {
			return;
		}

		tagStates[index].selected = !tagStates[index].selected;
		if (tagStates[index].selected) {
			selectedTags.push(tagStates[index].tag);
		} else {
			selectedTags = selectedTags.filter(tag => tag != tagStates[index].tag);
		}

		displayedProjects = projects.filter(project => {
			for (let i = 0; i < selectedTags.length; i++) {
				if (project.tags.includes(selectedTags[i])) {
					return true;
				}
			}
			return false;
		});

		if (!displayedProjects.includes(selectedProject)) {
			selectedProject = displayedProjects[0];
			selectCaption(selectedProject.captions[0].picture, selectedProject.captions[0].description);
		}
	}

	let selectedCaption = {
		picture: selectedProject.captions[0].picture,
		description: selectedProject.captions[0].description
	};

	let selectedCaptionIndex = 0;


	const selectCaption = (picture, description, index) => {
		selectedCaption = {picture:picture, description: description};
		selectedCaptionIndex = index;
	}

	const selectProject = (project) => {
		selectedProject = project;
		selectCaption(project.captions[0].picture, project.captions[0].description);
	}

	let displayedProjects = projects;

	const goToNextCaption = () => {
		if (selectedCaptionIndex < selectedProject.captions.length - 1) {
			selectedCaptionIndex++;
			selectedCaption = {picture:selectedProject.captions[selectedCaptionIndex].picture, description: selectedProject.captions[selectedCaptionIndex].description};
		} else {
			selectedCaptionIndex = 0;
			selectedCaption = {picture:selectedProject.captions[selectedCaptionIndex].picture, description: selectedProject.captions[selectedCaptionIndex].description};
		}
	}



</script>

<div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8 animate-quickerFadeIn">


<div>
	<ul role="list" class="mt-3 grid grid-cols-3 gap-5 sm:grid-cols-4 sm:gap-6 lg:grid-cols-6">


{#each tagStates as tag, index}

		<li class="col-span-1 flex rounded-md shadow-sm">
			<button on:click={() => {updateSelectedTags(index)}} class="flex flex-1 items-center justify-between truncate rounded-md border-b border border-gray-200 {tagStates[index].selected ? 'bg-blue-400' : 'bg-white'}">
				<div class="flex-1 truncate px-2 py-2 text-sm">
					<p class="font-medium text-gray-900">{tag.tag}</p>
				</div>
				<div class="flex-shrink-0 pr-2">
				</div>
			</button>
		</li>
	{/each}

	</ul>
</div>







<ul role="list" class="grid grid-cols-2 gap-x-4 gap-y-8 sm:grid-cols-3 sm:gap-x-6 lg:grid-cols-4 xl:gap-x-8 mt-12">

	{#each displayedProjects as project, index}
		<li class="relative">
			<div class="group aspect-h-7 aspect-w-10 block w-full overflow-hidden rounded-lg bg-gray-100 focus-within:ring-2 focus-within:ring-indigo-500 focus-within:ring-offset-2 focus-within:ring-offset-gray-100">
				<img src="{project.captions[0].picture}" alt="" class="pointer-events-none object-cover group-hover:opacity-75">
				<button on:click={() => {selectProject(project)}} type="button" class="absolute inset-0 focus:outline-none">
					<span class="sr-only">View details for {project.name}.HEIC</span>
				</button>
			</div>
		</li>
		{/each}

</ul>


	<div class="relative mt-12">
		<div class="absolute inset-0 flex items-center" aria-hidden="true">
			<div class="w-full border-t border-black"></div>
		</div>
		<div class="relative flex justify-center">
			<span class="bg-white px-3 text-base font-semibold leading-6 text-gray-900">Project View</span>
		</div>
	</div>






	<div class="bg-transparent">
		<div class="mx-auto max-w-2xl px-4 sm:px-6 sm:py-12 lg:max-w-7xl lg:px-8">
			<div class="lg:grid lg:grid-cols-2 lg:items-start lg:gap-x-8">
				<!-- Image gallery -->
				<div class="flex flex-col-reverse">
					<!-- Image selector -->
					<div class="mx-auto mt-6 hidden w-full max-w-2xl sm:block lg:max-w-none">
						<div class="grid grid-cols-4 gap-6" aria-orientation="horizontal" role="tablist">

							{#each selectedProject.captions as image, imageIndex}

								<button on:click={() => {selectCaption(image.picture, image.description, imageIndex)}} id="tabs-1-tab-1" class="relative flex h-24 cursor-pointer items-center justify-center rounded-md bg-white text-sm font-medium uppercase text-gray-900 hover:bg-gray-50 focus:outline-none focus:ring focus:ring-opacity-50 focus:ring-offset-4" aria-controls="tabs-1-panel-1" role="tab" type="button">
									<span class="sr-only">Angled view</span>
									<span class="absolute inset-0 overflow-hidden rounded-md">
                <img src="{image.picture}" alt="" class="h-full w-full object-cover object-center">
              </span>
									<!-- Selected: "ring-indigo-500", Not Selected: "ring-transparent" -->
									<span class="ring-transparent pointer-events-none absolute inset-0 rounded-md ring-2 ring-offset-2" aria-hidden="true"></span>
								</button>

								{/each}

						</div>
					</div>

					<div class="aspect-h-1 aspect-w-1 w-full">
						<!-- Tab panel, show/hide based on tab state. -->
						<div on:click={() => {goToNextCaption()}} on:keypress={() => {goToNextCaption()}} id="tabs-1-panel-1" aria-labelledby="tabs-1-tab-1" role="tabpanel" tabindex="0">
							<img src="{selectedCaption.picture}" alt="descpicture" class="h-full w-full object-contain object-center sm:rounded-lg">
						</div>

						<!-- More images... -->
					</div>
				</div>

				<!-- Product info -->
				<div class="mt-10 px-4 sm:mt-16 sm:px-0 lg:mt-0">
					<h1 class="text-3xl font-bold tracking-tight text-gray-900">{selectedProject.name}</h1>

					<div class="mt-6">
						<h3 class="sr-only">Description</h3>

						<div class="space-y-6 text-base text-gray-700">
							<p>{@html selectedProject.description.replace(/\n/g, '<br>')}</p>
							<br>
							<a href="{selectedProject.link}" class="text-blue-700">Link to resource</a>
						</div>
					</div>



					<section aria-labelledby="details-heading" class="mt-12">

						<div class="divide-y divide-gray-200 border-t">
							<div>
								<div class="prose prose-sm pb-6 mt-2" id="disclosure-1">
									<p>{selectedCaption.description}</p>
								</div>
							</div>
						</div>

					</section>
				</div>
			</div>
		</div>
	</div>




</div>
