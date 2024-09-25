<script>
    export let dropdownPosition;
    export let selectedItem;
    import { createEventDispatcher } from 'svelte';
  
    const dispatch = createEventDispatcher();
  
    function handleAction(action) {
      if (action === 'delete') {
        dispatch('delete');
      } else if (action === 'edit') {
        dispatch('edit');
      }
      console.log(`Action: ${action} on item: ${selectedItem}`);
    }
  </script>
  
  <div class="dropdown-menu" style="top: {dropdownPosition.y}px; left: {dropdownPosition.x}px;">
    <ul>
      <li>
        <button type="button" on:click={() => handleAction('edit')}>Edit</button>
      </li>
      <li>
        <button type="button" on:click={() => handleAction('delete')}>Delete</button>
      </li>
    </ul>
  </div>
  
  <style>
    .dropdown-menu {
      position: absolute;
      background: white;
      border: 1px solid #ccc;
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
      z-index: 1000;
    }
  
    .dropdown-menu ul {
      list-style: none;
      margin: 0;
      padding: 0;
    }
  
    .dropdown-menu li {
      padding: 8px 12px;
    }
  
    .dropdown-menu button {
      background: none;
      border: none;
      padding: 8px 12px;
      cursor: pointer;
      width: 100%;
      text-align: left;
    }
  
    .dropdown-menu button:hover {
      background-color: #f0f0f0;
    }
  </style>