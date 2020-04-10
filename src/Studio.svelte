<script>
    import { slide, scale } from 'svelte/transition'
    export let studio;
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
</style>

<div
    class="box"
    transition:scale={{
        delay: 100,
        duration: 400
    }}
>
    <button class="delete" on:click></button>
    <div class="field">
        <label class="label">Name:</label>
        <input type="text" class="input" bind:value={studio.name}>
    </div>
    <div class="field">
        <label class="label">Content:</label>
        {#each studio.content as paragraph }
            <div class="paragraph">
                <textarea class="textarea" bind:value={paragraph}></textarea>
                <button
                    class="delete"
                    on:click={()=>studio.content=[...studio.content.filter(c => c !== paragraph)]}
                />
            </div>
        {/each}
        <button
            class="button is-success is-outlined is-small"
            on:click={
                ()=>studio.content=[...studio.content, "new paragraph"]
            }>
            Add Paragraph
        </button>
    </div>
</div>