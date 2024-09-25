<!-- SelectedItemDisplay.svelte -->
<script>
    export let selectedItem = { name: '', tags: '' };
</script>

<div class="npc-identity">
    <h1>{selectedItem.name}</h1>
    <h2>{selectedItem.tags}</h2>
</div>

<style>
    .npc-identity {
        padding: 20px;
        border: 1px solid #ccc;
        border-radius: 8px;
        background: #f9f9f9;
    }
    .npc-identity h1 {
        margin: 0;
        font-size: 24px;
    }
    .npc-identity h2 {
        margin: 0;
        font-size: 18px;
        color: #666;
    }
</style>