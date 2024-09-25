<!-- new-image-modal.svelte -->
<script>
    import { createEventDispatcher } from 'svelte';

    let imageUrl = "";
    let file = null;
    const dispatch = createEventDispatcher();

    function handleSubmit() {
        if (file) {
            const reader = new FileReader();
            reader.onload = (e) => {
                dispatch('submit', { imageUrl: e.target.result });
            };
            reader.readAsDataURL(file);
        } else {
            dispatch('submit', { imageUrl });
        }
    }

    function handleCancel() {
        dispatch('cancel');
    }
</script>

<div class="new-image-modal">
    <div class="new-image-modal-content">
        <h2>Select Image</h2>
        <input type="text" placeholder="Enter image URL" bind:value={imageUrl} />
        <input type="file" accept="image/*" on:change={(e) => file = e.target.files[0]} />
        <button on:click={handleSubmit}>Submit</button>
        <button on:click={handleCancel}>Cancel</button>
    </div>
</div>

<style>
    .new-image-modal {
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
    .new-image-modal-content {
        background: white;
        padding: 20px;
        border-radius: 8px;
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    }
</style>