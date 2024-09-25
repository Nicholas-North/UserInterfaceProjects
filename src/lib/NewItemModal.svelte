<!-- NewItemModal.svelte -->
<script>
    import { createEventDispatcher } from 'svelte';

    let name = "";
    let tags = "";
    let dateCreated = "";
    const dispatch = createEventDispatcher();

    function handleSubmit() {
        dispatch('submit', { name, tags, dateCreated });
    }

    function handleCancel() {
        dispatch('cancel');
    }
</script>

<div class="modal">
    <div class="modal-content">
        <h2>Create New Item</h2>
        <input type="text" placeholder="Name" bind:value={name} />
        <input type="text" placeholder="Tags" bind:value={tags} />
        <input type="date" placeholder="Date Created" bind:value={dateCreated} />
        <button on:click={handleSubmit}>Submit</button>
        <button on:click={handleCancel}>Cancel</button>
    </div>
</div>

<style>
    .modal {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: rgba(0, 0, 0, 0.5);
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .modal-content {
        background: white;
        padding: 20px;
        border-radius: 8px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }
</style>