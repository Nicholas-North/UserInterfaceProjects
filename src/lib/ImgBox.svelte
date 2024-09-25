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
        width: 250px; /* Set your desired width */
        height: 250px; /* Set your desired height */
        border: 1px solid #ccc;
        display: flex;
        align-items: center;
        justify-content: center;
        overflow: hidden; /* Ensure content does not overflow */
    }

    .imgbox-image {
        max-width: 100%;
        max-height: 100%;
        object-fit: contain; /* Ensure the image fits within the box */
    }

    .imgbox-placeholder {
        display: flex;
        align-items: center;
        justify-content: center;
        width: 100%;
        height: 100%;
    }
    .imgbox-placeholder button {
        padding: 10px 20px;
        font-size: 16px;
        cursor: pointer;
    }
</style>