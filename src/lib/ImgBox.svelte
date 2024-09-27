<script>
    import NewImageModal from "./NewImageModal.svelte";

    export let selectedItem = { name: '', tags: '', dateCreated: '', description: '', image: '' };
    let altText = "Selected image"; // Alt text for the image
    let showModal = false;

    function handleImageSelected(event) {
        selectedItem.image = event.detail.imageUrl;
    }

    function openModal() {
        showModal = true;
    }

    function closeModal() {
        showModal = false;
    }

    function handleModalSubmit(event) {
        selectedItem.image = event.detail.imageUrl;
        closeModal();
    }
</script>

<div class="imgbox">
    {#if selectedItem.image}
        <div class="imgbox-container" on:click={openModal}>
            <img src={selectedItem.image} alt={altText} class="imgbox-image" />
            <div class="imgbox-overlay">
                <button class="imgbox-icon" on:click={openModal}>🔄</button>
            </div>
        </div>
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
        position: relative;
        border: 1px solid #ccc;
        border-radius: 8px;
        background: #f9f9f9;
        overflow: hidden;
    }
    .imgbox-container {
        position: relative;
        width: 100%;
        height: 100%;
    }
    .imgbox-image {
        width: 100%;
        height: 100%;
        object-fit: cover;
        transition: opacity 0.3s ease, filter 0.3s ease;
    }
    .imgbox-container:hover .imgbox-image {
        opacity: 0.7;
        filter: grayscale(50%);
    }
    .imgbox-overlay {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        display: flex;
        justify-content: center;
        align-items: center;
        background: rgba(0, 0, 0, 0.5);
        opacity: 0;
        transition: opacity 0.3s ease;
    }
    .imgbox-container:hover .imgbox-overlay {
        opacity: 1;
    }
    .imgbox-icon {
        width: 60px;
        height: 60px;
        border-radius: 50%;
        background: white;
        display: flex;
        justify-content: center;
        align-items: center;
        font-size: 30px;
        cursor: pointer;
        border: none;
        outline: none;
    }
    .imgbox-placeholder {
        display: flex;
        justify-content: center;
        align-items: center;
        width: 100%;
        height: 100%;
        border: 1px dashed #ccc;
        border-radius: 8px;
        background: #f0f0f0;
    }
</style>