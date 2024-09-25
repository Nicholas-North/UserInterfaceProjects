<!-- FilterBar.svelte -->
<script>
    import { createEventDispatcher } from 'svelte';

    let filterText = "";
    const dispatch = createEventDispatcher();

    function handleInput(event) {
        filterText = event.target.value;
        dispatch('filter', { filterText });
    }
</script>

<div class="filter-bar">
    <input type="text" placeholder="Enter text here" bind:value={filterText} on:input={handleInput} />
</div>

<style>
    .filter-bar {
        margin: 10px 0;
    }
    .filter-bar input {
        width: 100%;
        padding: 8px;
        font-size: 16px;
        border: 1px solid #ccc;
        border-radius: 4px;
    }
</style>