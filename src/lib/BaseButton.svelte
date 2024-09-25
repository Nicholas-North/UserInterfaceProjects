<script>
  import { createEventDispatcher } from 'svelte';
  export let text = 'Button';
  export let disabled = false;
  export let onClick = null;
  export let type = "button"; // Add type prop with default value "button"

  const dispatch = createEventDispatcher();

  function handleClick(event) {
    if (onClick && typeof onClick === 'function') {
      onClick(event); // Call the custom click handler if provided
    }
    dispatch('click', event);
  }
</script>

<button type='button' disabled={disabled} on:click={handleClick} class="button">
  <slot>{text}</slot> <!-- Use slot to allow custom content -->
</button>

<style>
  .button {
    padding: 0.5rem 1rem;
    border: none;
    border-radius: 4px;
    background-color: #007bff;
    color: white;
    cursor: pointer;
  }
  .button:disabled {
    background-color: #ccc;
    cursor: not-allowed;
  }
</style>