<script>
  // import uuid from "uuid-v4";
  import { v4 as uuidv4 } from "uuid";
  import { beforeUpdate, afterUpdate, onMount } from "svelte";

  let todos = [];

  onMount(() => {
    todos = JSON.parse(localStorage.getItem("todos")) || [];
  });

  let todoText, todoEditText;
  let isCompleted,
    isActive = false;

  const saveToStorage = () => {
    localStorage.setItem("todos", JSON.stringify(todos));
  };

  const handleAddTodo = (e) => {
    if (todoText) {
      todos = [...todos, { id: uuidv4(), completed: false, text: todoText }];
      todoText = "";
      saveToStorage();
    }
  };

  const handleDeleteTodo = (id) => {
    todos = todos.filter((todo) => todo.id !== id);
    saveToStorage();
  };

  const handleEdit = (e, id) => {
    const editedTodoText = e.target.textContent.trim();

    if (e.key === "Enter" || e.type === "blur") {
      e.target.blur();
      // on empty input str set element text back to the original todo text
      if (editedTodoText.length === 0) {
        todos.forEach((todo) => {
          if (todo.id === id) e.target.textContent = todo.text;
        });
        return;
      }

      todos = todos.map((todo) => {
        if (todo.id === id) {
          todo.text = editedTodoText;
          return todo;
        }
        return todo;
      });

      saveToStorage();
    }
  };

  const toggleCompleated = () => {
    isCompleted = true;
    isActive = false;
  };

  const toggleActive = () => {
    isActive = true;
    isCompleted = false;
  };

  const toggleAll = () => {
    isActive = false;
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

  .todos {
    width: 100%;
    margin-top: 20px;
  }

  .todo-item {
    display: flex;
    align-items: center;
    width: 100%;
    margin-bottom: 20px;
  }

  .todo-text {
    margin-left: 20px;
  }

  .todo-delete {
    margin-left: auto;
  }

  form {
    width: 100%;
    display: flex;
  }

  .new-todo {
    width: 100%;
    flex: 1;
  }

  .completed {
    text-decoration: line-through;
    color: grey;
  }

  .hidden {
    display: none;
  }

  .filters {
    display: flex;
    justify-content: space-between;
    width: 100%;
    margin-top: 20px;
  }
</style>

<div class="container">
  <h1>Your Todos, make them happen</h1>
  <form on:submit|preventDefault={handleAddTodo}>
    <input bind:value={todoText} class="new-todo" placeholder="Add new todo" />
    <button type="submit">Add todo</button>
  </form>
  <div class="todos">

    {#each todos as todo (todo.id)}
      <div
        class={isActive && todo.completed ? 'hidden' : isCompleted && !todo.completed ? 'hidden' : 'todo-item'}>
        <input
          type="checkbox"
          bind:checked={todo.completed}
          class="checkbox"
          on:change={() => saveToStorage()} />
        <div
          class="todo-text"
          class:completed={todo.completed}
          contenteditable="true"
          on:keydown={(e) => handleEdit(e, todo.id)}
          on:blur={(e) => handleEdit(e, todo.id)}>
          {todo.text}
        </div>
        <button class="todo-delete" on:click={() => handleDeleteTodo(todo.id)}>
          Delete todo
        </button>
      </div>
    {/each}

  </div>
  <div class="filters">
    <button on:click={toggleCompleated}>Completed</button>
    <button on:click={toggleActive}>Active</button>
    <button on:click={toggleAll}>All</button>
  </div>
</div>
