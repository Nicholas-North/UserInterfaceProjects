<script>
  export let items = [];

  // Extract column names from the first item in the array
  let columns = items.length > 0 ? Object.keys(items[0]) : [];

  // State for sorting
  let sortColumn = columns[0]; // Default to the first column
  let sortAscending = true; // Default to ascending order

  // Function to sort items by a given column
  function sortItems(column) {
    if (sortColumn === column) {
      sortAscending = !sortAscending; // Reverse the order if already sorting by this column
    } else {
      sortColumn = column;
      sortAscending = true; // Default to ascending order
    }

    items = [...items].sort((a, b) => {
      if (a[column] < b[column]) return sortAscending ? -1 : 1;
      if (a[column] > b[column]) return sortAscending ? 1 : -1;
      return 0;
    });
  }

  // Function to handle keydown events for accessibility
  function handleKeydown(event, column) {
    if (event.key === 'Enter' || event.key === ' ') {
      event.preventDefault();
      sortItems(column);
    }
  }
</script>

<div class="npc-item-array">
  <!-- Header Row -->
  <div class="header-row">
    {#each columns as column}
      <div class="item-column" role="button" tabindex="0" on:click={() => sortItems(column)} on:keydown={(event) => handleKeydown(event, column)}>
        <strong>{column}</strong>
      </div>
    {/each}
  </div>

  <!-- Data Rows -->
  <div class="data-rows">
    {#each items as item}
      <div class="item-row">
        {#each columns as column}
          <div class="item-column">
            {item[column]}
          </div>
        {/each}
      </div>
    {/each}
  </div>
</div>

<style>
  .npc-item-array {
    max-height: 400px; /* Adjust the height as needed */
    overflow-y: auto;
    border: 1px solid #ccc;
    border-radius: 4px;
    position: relative;
  }
  .header-row {
    display: flex;
    flex-direction: row;
    background-color: #f0f0f0;
    font-weight: bold;
    position: sticky;
    top: 0;
    z-index: 1;
    padding: 0.5rem;
    border-bottom: 1px solid #ccc;
  }
  .header-row .item-column {
    border-right: 1px solid #ccc; /* Add right border to header columns */
    transition: background-color 0.3s ease; /* Smooth transition for hover effect */
    cursor: pointer; /* Change cursor to pointer for header columns */
  }
  .header-row .item-column:last-child {
    border-right: none; /* Remove right border from the last header column */
  }
  .header-row .item-column:hover {
    background-color: #e0e0e0; /* Slightly darken the background on hover */
  }
  .data-rows {
    display: flex;
    flex-direction: column;
  }
  .item-row {
    display: flex;
    flex-direction: row;
    margin-bottom: 0.5rem;
    transition: background-color 0.3s ease; /* Smooth transition for hover effect */
  }
  .item-row:hover {
    background-color: #e0e0e0; /* Slightly darken the background on hover */
  }
  .item-column {
    flex: 1;
    padding: 0.5rem;
    border-right: 1px solid #ccc;
  }
  .item-column:last-child {
    border-right: none;
  }
</style>