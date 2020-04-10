<script>
    export let project;
    import { slide, scale } from 'svelte/transition'

    let current = "general"
</script>

<style>
div.box {
    position:relative
}
button.delete {
    position: absolute;
    top: 10px;
    right: 10px;
}

div.paragraph {
    position: relative;
    margin-top: 10px;
    margin-bottom: 10px;
}

div.link {
    position: relative;
    margin-top: 10px;
    margin-bottom: 10px;
    padding-top: 10px;
    padding-bottom: 10px;
    border-top: 1px solid #eee;
    border-bottom: 1px solid #eee;
}

div.project-tag {
    position: relative;
    margin-top: 10px;
    margin-bottom: 10px;
}


</style>

<div
    class="box"
    transition:scale={{
        delay: 100,
        duration: 400
    }}
>
    <div class="tabs is-small">
        <ul>
            <li class={ current==="general" ? "is-active" : ""}><a on:click={()=>current="general"}>General</a></li>
            <li class={ current==="description" ? "is-active" : ""}><a on:click={()=>current="description"}>Description</a></li>
            <li class={ current==="collaborators" ? "is-active" : ""}><a on:click={()=>current="collaborators"}>Collaborators</a></li>
            <li class={ current==="clients" ? "is-active" : ""}><a on:click={()=>current="clients"}>Clients</a></li>
            <li class={ current==="sponsors" ? "is-active" : ""}><a on:click={()=>current="sponsors"}>Sponsors</a></li>
            <li class={ current==="images" ? "is-active" : ""}><a on:click={()=>current="images"}>Images</a></li>
            <li class={ current==="tags" ? "is-active" : ""}><a on:click={()=>current="tags"}>Tags</a></li>
            <li class={ current==="links" ? "is-active" : ""}><a on:click={()=>current="links"}>Links</a></li>
        </ul>
    </div>

    <button class="delete" on:click></button>

    {#if current === "general"}

    <div class="field">
        <label class="label">Name:</label>
        <input type="text" class="input" bind:value={project.name}>
    </div>
    <div class="field">
        <label class="label">Slug:</label>
        <input type="text" class="input" bind:value={project.slug}>
    </div>
    <div class="field">
        <label class="label">Year:</label>
        <input type="number" class="input" bind:value={project.year}>
    </div>

    {:else if current === "description"}

    <div class="field">
        <label class="label">Description:</label>
        {#each project.description as paragraph }
            <div class="paragraph">
                <textarea class="textarea" bind:value={paragraph}></textarea>
                <button
                    class="delete"
                    on:click={
                        () => project.description = [...project.description.filter(p => p !== paragraph)]
                    }
                />
            </div>
        {/each}
        <button
            class="button is-success is-outlined is-small"
            on:click={
                () => project.description = [...project.description, "new paragraph"]
            }>
            Add Description
        </button>
    </div>

    {:else if current === "collaborators"}

    <div class="field">
        <label class="label">Collaborators:</label>
        {#each project.collaborators as link }
            <div class="link">
                <div class="field">
                    <label class="label">Name:</label>
                    <input type="text" class="input" bind:value={link.name}>
                </div>
                <div class="field">
                    <label class="label">URL:</label>
                    <input type="text" class="input" bind:value={link.url}>
                </div>
                <button
                    class="delete"
                    on:click={
                        () => project.collaborators = [...project.collaborators.filter(l => l !== link)]
                    }
                />
            </div>
        {/each}
        <button
            class="button is-success is-outlined is-small"
            on:click={
                () => project.collaborators = [...project.collaborators, {
                    name:"name",
                    url:"url"
                }]
            }>Add Collaborator</button>
    </div>

    {:else if current === "clients"}

    <div class="field">
        <label class="label">Clients:</label>
        {#each project.clients as link }
            <div class="link">
                <div class="field">
                    <label class="label">Name:</label>
                    <input type="text" class="input" bind:value={link.name}>
                </div>
                <div class="field">
                    <label class="label">URL:</label>
                    <input type="text" class="input" bind:value={link.url}>
                </div>
                <button
                    class="delete"
                    on:click={
                        () => project.clients = [...project.clients.filter(l => l !== link)]
                    }
                />
            </div>
        {/each}
        <button
            class="button is-success is-outlined is-small"
            on:click={
                () => project.clients=[...project.clients, {
                    name: "name",
                    url: "url"
                }]
            }>
            Add Client
        </button>
    </div>

    {:else if current === "sponsors"}

    <div class="field">
        <label class="label">Sponsors:</label>
        {#each project.sponsors as link }
            <div class="link">
                <div class="field">
                    <label class="label">Name:</label>
                    <input type="text" class="input" bind:value={link.name}>
                </div>
                <div class="field">
                    <label class="label">URL:</label>
                    <input type="text" class="input" bind:value={link.url}>
                </div>
                <button
                    class="delete"
                    on:click={
                        () => project.sponsors = [...project.sponsors.filter(l => l !== link)]
                    }
                />
            </div>
        {/each}
        <button
            class="button is-success is-outlined is-small"
            on:click={
                () => project.sponsors = [...project.sponsors, {
                    name:"name",
                    url:"url"
                }]
            }>
            Add Sponsor
        </button>
    </div>

    {:else if current === "images"}

    <div class="field">
        <label class="label">Images:</label>
        <div class="link">
            {#each project.images as image }
                <div class="project-tag">
                    <input type="text" class="input project-tag" bind:value={image}>
                    <button
                        class="delete"
                        on:click={
                            () => project.images = [...project.images.filter(i => i !== image)]
                        }
                    />
                </div>
            {/each}
        </div>
        <button
            class="button is-success is-outlined is-small"
            on:click={
                () => project.images = [...project.images, "new image"]
            }>
            Add Tag
        </button>
    </div>


    {:else if current === "tags"}

    <div class="field">
        <label class="label">Tags:</label>
        <div class="link">
            {#each project.tags as tag }
                <div class="project-tag">
                    <input type="text" class="input project-tag" bind:value={tag}>
                    <button
                        class="delete"
                        on:click={
                            () => project.tags = [...project.tags.filter(t => t !== tag)]
                        }
                    />
                </div>
            {/each}
        </div>
        <button
            class="button is-success is-outlined is-small"
            on:click={
                () => project.tags=[...project.tags, "new tag"]
            }
            >Add Tag
        </button>
    </div>

    {:else if current === "links"}

    <div class="field">
        <label class="label">Links:</label>
        <div class="link">
            {#each project.links as link }
                <div class="project-tag">
                    <input type="text" class="input project-tag" bind:value={link}>
                    <button
                        class="delete"
                        on:click={
                            () => project.links = [...project.links.filter(l => l !== link)]
                        }
                    />
                </div>
            {/each}
        </div>
        <button
            class="button is-success is-outlined is-small"
            on:click={
                () => project.links = [...project.links, "new link"]
            }>
            Add Link
        </button>
    </div>

    {/if}
</div>