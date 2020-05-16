<script>
  export let todo,
    isCompleted,
    isUncompleated,
    saveToStorage,
    handleEditTodo,
    handleDeleteTodo;
</script>

<style>

</style>

<div
  class={isUncompleated && todo.completed ? 'hidden' : isCompleted && !todo.completed ? 'hidden' : 'todo-item'}>
  <input
    type="checkbox"
    bind:checked={todo.completed}
    class="checkbox"
    on:change={() => saveToStorage()} />
  <div
    class="todo-text"
    class:completed={todo.completed}
    contenteditable="true"
    on:keydown={(e) => handleEditTodo(e, todo.id)}
    on:blur={(e) => handleEditTodo(e, todo.id)}>
    {todo.text}
  </div>
  <button class="todo-delete" on:click={() => handleDeleteTodo(todo.id)}>
    Delete todo
  </button>
</div>
