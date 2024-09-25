<script>
    export let options = [];
    export let selectedValue = '';
  
    function handleChange(event) {
      selectedValue = event.target.value;
      dispatch('change', selectedValue);
    }
  
    import { createEventDispatcher } from 'svelte';
    const dispatch = createEventDispatcher();
  </script>
  
  <div class="radial-buttons">
    {#each options as option}
      <label>
        <input
          type="radio"
          name="radial-buttons"
          value={option}
          bind:group={selectedValue}
          on:change={handleChange}
        />
        {option}
      </label>
    {/each}
  </div>
  
  <!-- <style>
    .radial-buttons {
      display: flex;
      flex-direction: column;
    }
  
    label {
      margin-bottom: 0.5rem;
    }
  </style> -->