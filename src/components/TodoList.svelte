<script>
  import TodoItem from "./TodoItem.svelte";
  export let todos,
    isCompleted,
    isUncompleated,
    saveToStorage,
    handleEditTodo,
    handleDeleteTodo;
</script>

<style>

</style>

<div class="todos">

  {#each todos as todo (todo.id)}
    <TodoItem
      {todo}
      {isCompleted}
      {isUncompleated}
      {saveToStorage}
      {handleEditTodo}
      {handleDeleteTodo} />
  {/each}

</div>
