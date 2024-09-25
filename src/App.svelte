<script>
  import { onMount } from 'svelte';
  import Button from './lib/BaseButton.svelte';
  import BaseItemArray from './lib/NPCItemArray.svelte';
  import ImgBox from './lib/ImgBox.svelte';
  import FilterBar from './lib/FilterBar.svelte';
  import NewItemModal from './lib/NewItemModal.svelte';
  import SelectedItemDisplay from './lib/SelectedItemDisplay.svelte';
  import DropDownMenu from './lib/DropDownMenu.svelte';

  let items = [
    { name: 'Project Alpha', tags: 'urgent, client', dateCreated: '2023-01-15' },
    { name: 'Project Beta', tags: 'internal, low-priority', dateCreated: '2023-02-20' },
    { name: 'Project Gamma', tags: 'client, high-priority', dateCreated: '2023-03-05' },
    { name: 'Project Delta', tags: 'internal, medium-priority', dateCreated: '2023-04-10' },
    { name: 'Project Epsilon', tags: 'urgent, internal', dateCreated: '2023-05-25' },
    { name: 'Project Zeta', tags: 'client, low-priority', dateCreated: '2023-06-30' },
    { name: 'Project Eta', tags: 'high-priority, internal', dateCreated: '2023-07-15' },
    { name: 'Project Theta', tags: 'client, medium-priority', dateCreated: '2023-08-20' },
    { name: 'Project Iota', tags: 'urgent, high-priority', dateCreated: '2023-09-05' },
    { name: 'Project Kappa', tags: 'internal, low-priority', dateCreated: '2023-10-10' }
  ];
  let filteredItems = items;
  let showModal = false;
  let selectedItem = items[0]; // Default to the first item
  let dropdownVisible = false;
  let dropdownPosition = { x: 0, y: 0 };
  let isEditing = false;
  let itemBeingEdited = null;
  let updatedItem = {};

  $: filteredItems = items;

  function handleFilter(event) {
    const filterText = event.detail.filterText.toLowerCase();
    filteredItems = items.filter(item => 
      Object.values(item).some(value => 
        value.toString().toLowerCase().includes(filterText)
      )
    );
  }

  function openModal() {
    showModal = true;
  }

  function closeModal() {
    showModal = false;
  }

  function handleItemClick(event) {
    selectedItem = event.detail.item;
    isEditing = false; // Reset editing state when a new item is selected
  }

  function handleContextMenu(event) {
    event.preventDefault();
    dropdownPosition = { x: event.detail.event.clientX, y: event.detail.event.clientY };
    dropdownVisible = true;
    selectedItem = event.detail.item;
  }

  function closeDropdown() {
    dropdownVisible = false;
  }

  function deleteSelectedItem() {
    items = items.filter(item => item !== selectedItem);
    filteredItems = items; // Update the filtered items as well
    dropdownVisible = false; // Close the dropdown after deletion
  }
  
  function handleModalSubmit(event) {
    const newItem = event.detail;
    if (isEditing) {
      saveChanges(newItem);
    } else {
      items = [...items, newItem];
      filteredItems = items; // Update the filtered items as well
    }
    closeModal();
  }
  function toggleEdit() {
    isEditing = true;
    itemBeingEdited = selectedItem;
    updatedItem = { ...selectedItem }; // Initialize updatedItem with the selected item data
    dropdownVisible = false; // Close the dropdown after toggling edit
  }

  function saveChanges() {
    const index = items.findIndex(item => item === itemBeingEdited);
    if (index !== -1) {
      items[index] = { ...updatedItem }; // Update the existing item
    }
    filteredItems = items.map(item => ({ ...item })); // Update the filtered items with deep copies
    selectedItem = { ...updatedItem }; // Update the selected item with a deep copy
    isEditing = false;
    itemBeingEdited = null;

    // Trigger a re-render by updating a reactive variable
    items = [...items];
    filteredItems = [...filteredItems];
  }

  onMount(() => {
    window.addEventListener('click', closeDropdown);
    return () => {
      window.removeEventListener('click', closeDropdown);
    };
  });
</script>

<main>
  <body>
    <div class="main-body-elements">
      <div class="container-one">
        <div class="taskbar">
          <Button text="New" onClick={openModal}></Button>
          <FilterBar on:filter={handleFilter} />
        </div>
        <BaseItemArray items={filteredItems} on:itemClick={handleItemClick} on:contextmenu={handleContextMenu} />
      </div>
      <div class="container-two">
        <div class="npc-header">
          {#if isEditing}
            <div>
              <input type="text" bind:value={updatedItem.name} />
              <input type="text" bind:value={updatedItem.tags} />
              <input type="text" bind:value={updatedItem.dateCreated} />
              <button on:click={saveChanges}>Save</button>
            </div>
          {:else}
            <SelectedItemDisplay {selectedItem} />
          {/if}
          <!-- <ImgBox imageUrl="src/images/HPY.png" /> -->
          <ImgBox imageUrl="" />
        </div>
        <p></p>
      </div>
    </div>
    {#if dropdownVisible}
      <DropDownMenu {dropdownPosition} {selectedItem} on:delete={deleteSelectedItem} on:edit={toggleEdit} />
    {/if}
  </body>
</main>

{#if showModal}
    <NewItemModal on:submit={handleModalSubmit} on:cancel={closeModal} {isEditing} {itemBeingEdited} />
{/if}

<style>
  .main-body-elements {
    display: flex;
    flex-direction: row;
    justify-content: flex-start; /* Ensure children are not stretched */
    gap: 1rem; /* Add some space between the containers */
  }
  .container-one, .container-two {
    display: inline-flex;
    flex-direction: column;
    gap: 1rem;
    padding: 1rem;
    border: 1px solid #ccc;
    border-radius: 8px;
    background-color: #f0f0f0;
    max-width: fit-content; /* Ensure the container only extends to fit its content */
  }
  .taskbar {
    display: inline-flex;
    align-items: center;
    gap: 1rem;
    padding: 1rem;
    border: 1px solid #ccc;
    border-radius: 8px;
    background-color: #f0f0f0;
  }
  .npc-header {
    display: inline-flex;
    align-items: center;
    border: 1px solid #ccc;
    border-radius: 8px;
    background-color: #f0f0f0;
  }
</style>