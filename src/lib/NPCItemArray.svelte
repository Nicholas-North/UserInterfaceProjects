<script>
  import { createEventDispatcher } from 'svelte';

  export let items = [];
  export let sortColumn = '';
  export let sortAscending = true;

  const dispatch = createEventDispatcher();

  // State for selected item
  let selectedItem = null;

  // Function to sort items by a given column
  function sortItems(column) {
    if (sortColumn === column) {
      sortAscending = !sortAscending; // Reverse the order if already sorting by this column
    } else {
      sortColumn = column;
      sortAscending = true; // Default to ascending order
    }

    items = [...items].sort((a, b) => {
      let aValue = a[column];
      let bValue = b[column];

      // Handle date comparison if the column is dateCreated
      if (column === 'dateCreated') {
        aValue = new Date(aValue);
        bValue = new Date(bValue);
      }

      if (aValue < bValue) return sortAscending ? -1 : 1;
      if (aValue > bValue) return sortAscending ? 1 : -1;
      return 0;
    });
  }

  // Function to handle item click
  function handleItemClick(item) {
    selectedItem = item;
    dispatch('itemClick', { item });
  }
</script>
<style>
  table {
    width: 100%;
    border-collapse: collapse;
  }

  th, td {
    border: 1px solid #ddd;
    padding: 8px;
    text-align: left;
  }

  th {
    background-color: #f2f2f2;
    cursor: pointer;
  }

  tr:nth-child(even) {
    background-color: #f9f9f9;
  }

  tr:hover {
    background-color: #ddd;
  }

  tr.selected {
    background-color: #b3d9ff;
  }
</style>

<table>
  <thead>
    <tr>
      <th on:click={() => sortItems('name')}>Name</th>
      <th on:click={() => sortItems('tags')}>Tags</th>
      <th on:click={() => sortItems('dateCreated')}>Date Created</th>
    </tr>
  </thead>
  <tbody>
    {#each items as item}
      <tr class:selected={item === selectedItem} on:click={() => handleItemClick(item)}>
        <td>{item.name}</td>
        <td>{item.tags}</td>
        <td>{item.dateCreated}</td>
      </tr>
    {/each}
  </tbody>
</table>