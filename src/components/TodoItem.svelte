<script>
  import { createEventDispatcher } from 'svelte';
  export let todo, isCompleted, isUncompleated;

  const dispatch = createEventDispatcher();

  const handleDeleteTodo = (todoId) => {
    dispatch('deleteTodo', todoId);
  };

  const handleEditTodo = (event, todoId) => {
    const { value } = event.target;
    dispatch('editTodo', {
      event,
      todoId,
      value,
    });
  };

  const handleChecked = (todoId) => {
    dispatch('checkTodo', {
      todoId,
    });
  };
</script>

<style>
  .todo-item {
    display: flex;
    align-items: center;
    /* width: 100%;
    margin-bottom: 20px;
    background-color: white;
    border-radius: 6px;
    box-shadow: 0 0.5em 1em -0.125em rgba(10, 10, 10, 0.1),
      0 0px 0 1px rgba(10, 10, 10, 0.02);
    color: #4a4a4a;
    padding: 1.25rem; */
  }
  .todo-text {
    flex: 1;
    padding: 5px;
    margin-left: 10px;
    border: none;
    box-shadow: none;
  }

  .todo-text:active,
  .todo-text:focus {
    box-shadow: none;
    border: none;
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
  class={isUncompleated && todo.completed ? 'hidden' : isCompleted && !todo.completed ? 'hidden' : 'todo-item box'}>
  <input
    type="checkbox"
    class="checkbox"
    on:change={() => handleChecked(todo.id)}
    bind:checked={todo.completed} />
  <input
    class="todo-text is-size-5 input"
    class:completed={todo.completed}
    value={todo.text}
    on:keydown={(e) => handleEditTodo(e, todo.id)}
    on:blur={(e) => handleEditTodo(e, todo.id)} />
  <button class="todo-delete button" on:click={() => handleDeleteTodo(todo.id)}>
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
