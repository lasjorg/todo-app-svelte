<script>
  export let todoText;
  export let handleAddTodo;
</script>

<style>

</style>

<form on:submit|preventDefault={handleAddTodo}>
  <input bind:value={todoText} class="new-todo" placeholder="Add new todo" />
  <button type="submit">Add todo</button>
</form>
