<!-- SelectedItemDisplay.svelte -->
<script>
    import ImgBox from "./ImgBox.svelte";
    export let selectedItem = { name: '', tags: '', dateCreated: '', description: '', image: '' };
    export let editMode = false;
    $: console.log('Selected item:', selectedItem);
    $: console.log('Edit mode:', editMode);
</script>

{#if !editMode}
    <div class="npc-overall">
        <div class="npc-identity">
            {#if selectedItem}
                <div>
                    <h1>{selectedItem.name}</h1>
                    <h2>{selectedItem.tags}</h2>
                    <h3>Date-Created: {selectedItem.dateCreated}</h3>
                </div>
                <ImgBox selectedItem={selectedItem} />
            {:else}
                <div>
                <p>No item selected</p>
                </div>
            {/if}
        </div>
        <p class="npc-description">{selectedItem.description}</p>
    </div>
{:else}
    <div class="npc-overall">
        <div class="npc-identity">
            <div>
                <input type="text" bind:value={selectedItem.name} placeholder={selectedItem.name} />
                <input type="text" bind:value={selectedItem.tags} placeholder={selectedItem.tags} />
                <input type="text" bind:value={selectedItem.dateCreated} placeholder={selectedItem.dateCreated} />
            </div>
            <ImgBox {selectedItem} />
        </div>
        <input type="text" bind:value={selectedItem.description} placeholder={selectedItem.description} class="npc-description-input"/>
    </div>
    <button on:click={() => editMode = false}>Save</button>
{/if}


<style>
    .npc-overall {
        display: flex;
        flex-direction: column;
        gap: 10px;
        width: 100%;
        height: 100%;
    }
    .npc-identity {
        display: flex;
        align-items: center;
        gap: 20px;
        width: 100%;
    }
    .npc-details {
        display: flex;
        flex-direction: column;
        gap: 5px;
        flex-grow: 1;
    }
    .npc-description {
        flex-grow: 1;
        width: 100%;
    }
    .npc-description-input {
        width: 100%;
        padding: 5px;
        border: 1px solid #ccc;
        border-radius: 4px;
        box-sizing: border-box;
    }
    input {
        padding: 5px;
        border: 1px solid #ccc;
        border-radius: 4px;
        box-sizing: border-box;
        width: 100%;
    }
</style>