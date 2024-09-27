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
    { name: 'Happy', tags: 'Human, Rogue, NPC', dateCreated: '2023-01-15', description: 'Happy is a human rogue, who hails from the town of neverwinter', image: 'src/images/HPY.png' },
    { name: 'Grumpy', tags: 'Dwarf, Warrior, NPC', dateCreated: '2023-02-20', description: 'Grumpy is a dwarf warrior, known for his short temper and strength', image: 'https://via.placeholder.com/150' },
    { name: 'Sleepy', tags: 'Elf, Mage, NPC', dateCreated: '2023-03-10', description: 'Sleepy is an elf mage, who is always seen with a sleepy expression', image: 'https://via.placeholder.com/150' },
    { name: 'Bashful', tags: 'Halfling, Bard, NPC', dateCreated: '2023-04-05', description: 'Bashful is a halfling bard, who is shy but has a beautiful singing voice', image: 'https://via.placeholder.com/150' },
    { name: 'Sneezy', tags: 'Orc, Shaman, NPC', dateCreated: '2023-05-15', description: 'Sneezy is an orc shaman, who is known for his constant sneezing', image: 'https://via.placeholder.com/150' },
    { name: 'Doc', tags: 'Gnome, Alchemist, NPC', dateCreated: '2023-06-25', description: 'Doc is a gnome alchemist, who is always experimenting with new potions', image: 'https://via.placeholder.com/150' },
    { name: 'Dopey', tags: 'Troll, Thief, NPC', dateCreated: '2023-07-30', description: 'Dopey is a troll thief, who is clumsy but surprisingly effective', image: 'https://via.placeholder.com/150' },
    { name: 'Jolly', tags: 'Human, Merchant, NPC', dateCreated: '2023-08-10', description: 'Jolly is a human merchant, who is always cheerful and ready to make a deal', image: 'https://via.placeholder.com/150' },
    { name: 'Gloomy', tags: 'Elf, Ranger, NPC', dateCreated: '2023-09-15', description: 'Gloomy is an elf ranger, who prefers solitude and the company of nature', image: 'https://via.placeholder.com/150' },
    { name: 'Cheery', tags: 'Dwarf, Miner, NPC', dateCreated: '2023-10-01', description: 'Cheery is a dwarf miner, who is always in high spirits despite the hard work', image: 'https://via.placeholder.com/150' },
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

  function handleContextMenu(event, item) {
    event.preventDefault();
    dropdownPosition = { x: event.clientX, y: event.clientY };
    dropdownVisible = true;
    selectedItem = item;
  }

  function closeDropdown() {
    dropdownVisible = false;
  }

  function deleteSelectedItem() {
    console.log('deleteSelectedItem called for item:', selectedItem);
    items = items.filter(item => item !== selectedItem);
    filteredItems = items; // Update the filtered items as well
    dropdownVisible = false; // Close the dropdown after deletion
    console.log('Items after deletion:', items);
  }
  
  function handleModalSubmit(event) {
    const newItem = event.detail;
    if (isEditing) {
      isEditing = false;
    } else {
      items = [...items, newItem];
      filteredItems = items; // Update the filtered items as well
    }
    closeModal();
  }
  function toggleEdit() {
    isEditing = true;
    dropdownVisible = false; // Close the dropdown after toggling edit
  }

  // function saveChanges() {
  //   const index = items.findIndex(item => item === itemBeingEdited);
  //   if (index !== -1) {
  //     items[index] = { ...updatedItem }; // Update the existing item
  //   }
  //   filteredItems = items.map(item => ({ ...item })); // Update the filtered items with deep copies
  //   selectedItem = { ...updatedItem }; // Update the selected item with a deep copy
  //   isEditing = false;
  //   itemBeingEdited = null;

  //   // Trigger a re-render by updating a reactive variable
  //   items = [...items];
  //   filteredItems = [...filteredItems];
  // }

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
        <BaseItemArray items={filteredItems} handleContextMenu={handleContextMenu} on:itemClick={handleItemClick} />
      </div>
      <div class="container-two">
        <div class="npc-header">
          <SelectedItemDisplay selectedItem={selectedItem} editMode={isEditing} />
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
    <NewItemModal on:submit={handleModalSubmit} on:cancel={closeModal} />
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