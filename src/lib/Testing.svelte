<script>
    import NewBaseModal from './NewBaseModal.svelte';

    let showModal = false;

    function openModal() {
        showModal = true;
    }

    function handleClose() {
        showModal = false;
    }

    function customAction() {
        alert('Custom action executed!');
        handleClose();
    }

    const buttons = [
        { text: 'Custom Action', action: customAction }
    ];
</script>

<button on:click={openModal}>Open Modal</button>

<NewBaseModal
    bind:showModal={showModal}
    header="Custom Modal Header"
    on:close={handleClose}
    {buttons}>
    <div slot="content">
        <p>This is custom content for the modal.</p>
    </div>
</NewBaseModal>