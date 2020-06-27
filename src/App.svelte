<script>
  import { v4 as uuidv4 } from 'uuid';
  import AddTodo from './components/AddTodo.svelte';
  import TodoList from './components/TodoList.svelte';
  import { beforeUpdate, afterUpdate, onMount } from 'svelte';

  let todos = [];
  let todoText;
  let isCompleted,
    isUncompleated = false;

  $: totalTodos = todos.length;
  $: totalCompleated = todos.filter((todo) => todo.completed).length;
  $: totalUncompleated = totalTodos - totalCompleated;

  onMount(() => {
    todos = JSON.parse(localStorage.getItem('todos')) || [];
  });

  const saveToStorage = () => {
    localStorage.setItem('todos', JSON.stringify(todos));
  };

  const handleAddTodo = ({ detail: { todoText } }) => {
    if (todoText) {
      todos = [...todos, { id: uuidv4(), completed: false, text: todoText }];
      todoText = '';
      saveToStorage();
    }
  };

  const handleDeleteTodo = (todoId) => {
    todos = todos.filter((todo) => todo.id !== todoId);
    saveToStorage();
  };

  const handleEditTodo = ({ event, todoId, value: todoText }) => {
    todoText = todoText.trim();

    if (event.key === 'Enter' || event.type === 'blur') {
      event.target.blur();
      // on empty input str set element text back to the original todo text
      if (todoText.length === 0) {
        todos.forEach((todo) => {
          if (todo.id === todoId) event.target.value = todo.text;
        });
        return;
      }

      todos = todos.map((todo) => {
        if (todo.id === todoId) {
          todo.text = todoText;
          return todo;
        }
        return todo;
      });

      saveToStorage();
    }
  };

  const handleChecked = ({ todoId }) => {
    todos = todos.map((todo) => {
      if (todo.id === todoId) {
        todo.completed = !todo.completed;
        return todo;
      }
      return todo;
    });
    saveToStorage();
  };

  const toggleCompleated = () => {
    isCompleted = true;
    isUncompleated = false;
  };

  const toggleUncompleated = () => {
    isUncompleated = true;
    isCompleted = false;
  };

  const toggleAll = () => {
    isUncompleated = false;
    isCompleted = false;
  };

  afterUpdate(() => {
    console.log(todos);
  });
</script>

<style>
  h1 {
    margin: 20px 0;
  }
  .container {
    width: 100%;
    max-width: 560px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 0 40px;
  }

  .filters {
    display: flex;
    justify-content: space-between;
    width: 100%;
    margin-top: 20px;
  }

  .filters button {
    display: flex;
    align-items: center;
  }
  .filters button > svg {
    margin-right: 5px;
  }
</style>

<div class="container">
  <h1>Svelte Todo App</h1>
  <AddTodo on:addTodo={handleAddTodo} />

  <TodoList
    {todos}
    {isCompleted}
    {isUncompleated}
    on:deleteTodo={({ detail }) => handleDeleteTodo(detail)}
    on:editTodo={({ detail }) => handleEditTodo(detail)}
    on:checkTodo={({ detail }) => handleChecked(detail)} />

  <h3>Todo list filters and stats</h3>
  <div class="filters">
    <button on:click={toggleCompleated}>
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
    <button on:click={toggleUncompleated}>
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
    <button on:click={toggleAll}>
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
    <!-- <button on:click={toggleCompleated}>Completed: {totalCompleated}</button>
    <button on:click={toggleUncompleated}>
      Uncompleated: {totalUncompleated}
    </button>
    <button on:click={toggleAll}>All: {totalTodos}</button> -->
  </div>
</div>
