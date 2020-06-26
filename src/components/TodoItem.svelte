<script>
  import { createEventDispatcher } from 'svelte';
  export let todo, isCompleted, isUncompleated;

  const dispatch = createEventDispatcher();

  const handleDeleteTodo = (todoId) => {
    dispatch('deleteTodo', todoId);
  };

  const handleEditTodo = (event, todoId) => {
    const { textContent } = event.target;
    dispatch('editTodo', {
      event,
      todoId,
      textContent,
    });
  };

  const handleChecked = (event, todoId) => {
    const isChecked = event.target.checked;

    dispatch('checkTodo', {
      isChecked,
      todoId,
    });
  };
</script>

<style>
  .todo-item {
    display: flex;
    align-items: center;
    width: 100%;
    margin-bottom: 20px;
    padding: 10px;
    background: rgba(0, 0, 0, 0.3);
  }
  .todo-text {
    margin-left: 10px;
    padding: 5px;
    flex: 1;
    outline: none;
    border: none;
    background: transparent;
  }
  .completed {
    text-decoration: line-through;
    color: grey;
  }

  .hidden {
    display: none;
  }

  .todo-delete {
    margin-left: auto;
    display: flex;
    padding: 2px;
  }
</style>

<div
  class={isUncompleated && todo.completed ? 'hidden' : isCompleted && !todo.completed ? 'hidden' : 'todo-item'}>
  <input
    type="checkbox"
    class="checkbox"
    on:change={(e) => handleChecked(e, todo.id)}
    bind:checked={todo.completed} />
  <div
    class="todo-text"
    class:completed={todo.completed}
    contenteditable="true"
    on:keydown={(e) => handleEditTodo(e, todo.id)}
    on:blur={(e) => handleEditTodo(e, todo.id)}>
    {todo.text}
  </div>
  <button class="todo-delete" on:click={() => handleDeleteTodo(todo.id)}>
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
      <line x1="18" y1="6" x2="6" y2="18" />
      <line x1="6" y1="6" x2="18" y2="18" />
    </svg>
  </button>
</div>
