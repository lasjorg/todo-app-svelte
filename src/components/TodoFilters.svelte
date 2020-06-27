<script>
  export let totalCompleated, totalUncompleated, totalTodos;

  import { createEventDispatcher } from 'svelte';
  const dispatch = createEventDispatcher();

  const handleToggleCompleated = () => {
    dispatch('toggleCompleated');
  };

  const handleToggleUncompleated = () => {
    dispatch('toggleUncompleated');
  };

  const handleToggleAll = () => {
    dispatch('toggleAll');
  };
</script>

<style>
  .filters {
    display: flex;
    justify-content: space-between;
    width: 100%;
    margin: 20px 0;
  }

  .filters button {
    display: flex;
    align-items: center;
  }
  .filters button > svg {
    margin-right: 5px;
  }
</style>

<h3 class="is-size-5">Todo list filters and stats</h3>
<div class="filters">
  <button class="button" on:click={handleToggleCompleated}>
    <svg
      viewBox="0 0 24 24"
      width="24"
      height="24"
      stroke="currentColor"
      stroke-width="2"
      fill="none"
      stroke-linecap="round"
      stroke-linejoin="round">
      <path d="M22 11.08V12a10 10 0 1 1-5.93-9.14" />
      <polyline points="22 4 12 14.01 9 11.01" />
    </svg>
    {totalCompleated}
  </button>
  <button class="button" on:click={handleToggleUncompleated}>
    <svg
      viewBox="0 0 24 24"
      width="24"
      height="24"
      stroke="currentColor"
      stroke-width="2"
      fill="none"
      stroke-linecap="round"
      stroke-linejoin="round">
      <circle cx="12" cy="12" r="10" />
    </svg>
    {totalUncompleated}
  </button>
  <button class="button" on:click={handleToggleAll}>
    <svg
      viewBox="0 0 24 24"
      width="24"
      height="24"
      stroke="currentColor"
      stroke-width="2"
      fill="none"
      stroke-linecap="round"
      stroke-linejoin="round"
      class="css-i6dzq1">
      <path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z" />
      <circle cx="12" cy="12" r="3" />
    </svg>
    {totalTodos}
  </button>
</div>
