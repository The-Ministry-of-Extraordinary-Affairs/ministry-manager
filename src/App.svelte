<script>
	import Nav from './Nav.svelte'
	import Studio from './Studio.svelte'
	import Project from './Project.svelte'

	import { slide, scale } from 'svelte/transition'

	let data = false;
	let studioVisible = true;
	let projectsVisible = true;

	let filepicker;

	$: numprojects = data.projects ? data.projects.length : "-"

	let message = {
		content: "",
		c: ""
	}
	let messageTimer;

	function msg(content, c) {
		clearTimeout(messageTimer)

		message = {
			content,
			c
		}
		content ? messageTimer = setTimeout(
			() => msg(), 3000
		) : ""
	}

	function newData() {
		data = {
			studio: [{
				name: "new text",
				content: ["New studio text."]
			}],
			projects: [{
				name: "new project",
				slug: "newproject",
				year: 2050,
				description: ["New project."],
				collaborators: [],
				clients: [],
				sponsors: [],
				images: [],
				tags: [],
				links: []
			}]
		}
	}

	function loadData() {
		console.log("uploading")
		msg("uploading", "is-info")
		let files = filepicker.files
		if (files.length <= 0) { msg("no file selected", "is-danger"); return }
		let fr = new FileReader()
		fr.onload = (e) => {
			data = JSON.parse(e.target.result)
		}
		fr.readAsText(files.item(0))
		msg(`Imported: ${files.item(0).name}`, "is-success")
	}

	function resetData() {
		data = false
	}

	function downloadData() {
		if (!data) { msg("Start with some data first, cowboy", "is-danger"); return }
		let a = document.createElement("a");
		let file = new Blob([JSON.stringify(data)]);
		a.href = URL.createObjectURL(file);
		a.download = 'data.json';
		a.click();
	}
</script>

<style>
	main {
		margin-left: auto;
		margin-right: auto;
	}

	h3 {
		display: flex;
		align-items: center;
	}

	.tag {
		margin-left: 10px;
	}

	h3 span.tag.hider {
		margin-left: auto;
		float: right;
	}

	div.flyout {
		margin-bottom: 30px;
	}
</style>

<div class="hero is-primary is-bold">
	<div class="hero-body">
		<div class="container">
			<h1 class="title">Ministry Content Editor</h1>
			<h2 class="subtitle">
				<span
					on:click={
						()=>msg("🙃 hello!", "is-primary")
					}>
					🚀
				</span>
				Edit and download your JSON.
			</h2>
		</div>
	</div>
</div>

<main class="section columns container">
	<div class="column is-one-quarter">
		{#if message.content }
			<article
				transition:slide={{
					delay: 100,
					duration: 400,
				}}
				class="message {message.c}"
			>
				<div class="message-body">
					{ message.content }
				</div>
			</article>
		{/if}
		<Nav
			bind:data
			on:load={loadData}
			on:start={newData}
			on:download={downloadData}
			on:reset={resetData}
			bind:filepicker={filepicker}
		/>
	</div>



	<div class="column">
		<h3 class="title">
			🏠 Studio
			<span class="tag hider" on:click={()=> studioVisible = !studioVisible}>
				{studioVisible ? "hide" : "show" }
			</span>
		</h3>
		{#if studioVisible }
		<div
			class="flyout"
			transition:slide={{
				duration: 400,
			}}
		>
		{#if data }
			{#each data.studio as studio (studio) }
				<Studio
					bind:studio
					on:click={
						()=>data.studio = [...data.studio.filter(s => s !== studio )]
					}
				/>
			{/each}
			<div class="buttons">
				<button
					class="button is-outlined is-success is-small"
					on:click={
						() => data.studio = [...data.studio, {
							name: "new text",
							content: ["New studio text."]
						}]
					}>
					Add Studio Text
				</button>
			</div>
		{:else}
		<div class="notification">
			No data has been loaded yet.
		</div>
		{/if}
		</div>
		{/if}
		<h3 class="title">
			🕹 Projects <span class="tag is-info is-light">{numprojects}</span>
			<span class="tag hider" on:click={()=> projectsVisible = !projectsVisible}>
				{ projectsVisible ? "hide" : "show" }
			</span>
		</h3>
		{#if projectsVisible}
		<div
			class="flyout"
			transition:slide={{
				duration: 400,
			}}
		>
		{#if data }
			{#each data.projects as project (project) }
				<Project
					bind:project
					on:click={
						()=>data.projects = [...data.projects.filter(p => p !== project)]
					}
				/>
			{/each}
			<div class="buttons">
				<button
					class="button is-outlined is-success is-small"
					on:click={
						() => data.projects = [...data.projects, {
							name: "new project",
							slug: "newproject",
							year: 2050,
							description: ["New project."],
							collaborators: [],
							clients: [],
							sponsors: [],
							images: [],
							tags: [],
							links: []
						}]
					}>
					Add Project
				</button>
			</div>
		{:else}
		<div class="notification">
			No data has been loaded yet.
		</div>
		{/if}
		</div>
		{/if}
	</div>
</main>