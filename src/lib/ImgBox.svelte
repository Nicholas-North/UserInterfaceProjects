<script>
    import NewImageModal from "./NewImageModal.svelte";

    let imageUrl = ""; // URL of the image
    let altText = "Selected image"; // Alt text for the image
    let showModal = false;

    function handleImageSelected(event) {
        imageUrl = event.detail.imageUrl;
    }

    function openModal() {
        showModal = true;
    }

    function closeModal() {
        showModal = false;
    }

    function handleModalSubmit(event) {
        imageUrl = event.detail.imageUrl;
        closeModal();
    }
</script>

<div class="imgbox">
    {#if imageUrl}
        <img src={imageUrl} alt={altText} class="imgbox-image" />
    {:else}
        <div class="imgbox-placeholder">
            <button on:click={openModal}>Select Image</button>
        </div>
    {/if}
</div>

{#if showModal}
    <NewImageModal on:submit={handleModalSubmit} on:cancel={closeModal} />
{/if}

<style>
    .imgbox {
        border: 1px solid #ccc;
        border-radius: 8px;
        overflow: hidden;
        max-width: 300px; /* Adjust as needed */
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        transition: transform 0.2s;
    }
    .imgbox-placeholder {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 200px; /* Adjust as needed */
    }
    .imgbox-placeholder button {
        padding: 10px 20px;
        font-size: 16px;
        cursor: pointer;
    }
</style>