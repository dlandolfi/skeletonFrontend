<script>
	let sections = [
		{ title: 'Chat', description: 'Chat that interacts with Go Websockets' },
		{ title: 'Quotes', description: 'Favorite quotes coming soon' },
		{ title: 'Stories', description: 'Stories on the way' }
	];
</script>

<div class="container mx-auto mt-8">
	<h2 class="text-2xl font-bold mx-4 mb-4">Things to look forward to</h2>

	{#each sections as section (section.title)}
		<a
			href="/{section.title.toLowerCase()}"
			class="block card card-hover mx-4 my-4"
			aria-label={section.title}
		>
			<header class="card-header text-xl">{section.title}</header>
			<section class="p-4">{section.description}</section>
		</a>
	{/each}
</div>

<style>
</style>
