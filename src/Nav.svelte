<script>
    import { createEventDispatcher } from 'svelte';
    const fire = createEventDispatcher();

    export let data;
    export let filepicker = undefined;
</script>

<div class="card">
    <div class="card-content">
        <p class="title">
            How to:
        </p>
        <p>
            💁‍♂️ You can select a file and upload it with the controls below. The data will appear to the right, edit and download your new data.
        </p>
        <hr />
        <p>
            ⚠️ The app does not check for correct formatting. Make sure your file is correctly formatted or start a new one.
        </p>
        <hr />

        {#if data }
        <div class="buttons has-addons is-centered">
            <button
                class="button is-small is-danger is-outlined"
                on:click={() => fire('reset')}>
                Reset
            </button>
            <button
                class="button is-small is-success"
                on:click={() => fire('download')}
            >
                Download
            </button>
        </div>

        {:else }
        <div class="file has-name is-fullwidth is-small is-info is-success">
            <label class="file-label">
                <input type="file" class="file-input" on:change={() => fire('load')} bind:this={filepicker} />
                <span class="file-cta">
                    <span class="file-label">
                        Choose a file...
                    </span>
                </span>
                <span class="file-name">
                    data.json
                </span>
            </label>
        </div>
        <div class="buttons is-right">
            <button class="button is-small is-info" on:click={() => fire('start')}>...or start from scratch</button>
        </div>
        {/if}
    </div>
</div>