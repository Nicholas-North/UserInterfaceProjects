<script>
  import svelteLogo from './assets/svelte.svg';
  import Button from './lib/BaseButton.svelte';
  import BaseItemArray from './lib/NPCItemArray.svelte';
  import ImgBox from './lib/ImgBox.svelte';
  import FilterBar from './lib/FilterBar.svelte';
  import NewItemModal from './lib/NewItemModal.svelte';
  import SelectedItemDisplay from './lib/SelectedItemDisplay.svelte';
  function openPopup() {
    window.open('https://example.com', '_blank', 'width=600,height=400'); // Open a new pop-up window
  }
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

  function handleModalSubmit(event) {
      items = [...items, event.detail];
      filteredItems = items;
      closeModal();
  }

  function handleItemClick(item) {
        selectedItem = item;
  }
</script>

<main>
<body>
  <div class="main-body-elements">
    <div class="container-one">
      <div class="taskbar">
        <Button text="New" onClick={openModal}></Button>
        <FilterBar on:filter={handleFilter} />
        <div class="radio-buttons">
          <label>
            <input type="radio" name="search" value="name" /> Name
          </label>
          <label>
            <input type="radio" name="search" value="tag" /> Tag
          </label>
        </div>
      </div>
      <BaseItemArray items={filteredItems} on:itemClick={handleItemClick} />
    </div>
    <div class="container-two">
      <div class="npc-header">
        <SelectedItemDisplay {selectedItem} />
        <!-- <ImgBox imageUrl="src/images/HPY.png" /> -->
        <ImgBox imageUrl="" />
      </div>
      <p></p>
    </div>
  </div>
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
  .radio-buttons {
    display: flex;
    flex-direction: column;
  }
  .npc-header {
    display: inline-flex;
    align-items: center;
    border: 1px solid #ccc;
    border-radius: 8px;
    background-color: #f0f0f0;
  }
</style>