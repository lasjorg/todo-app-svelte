<script>
  import { createEventDispatcher } from "svelte";
  let todoText;
  // export let handleAddTodo;

  const dispatch = createEventDispatcher();

  const handleAddTodo = () => {
    dispatch("addTodo", {
      todoText,
    });
    todoText = "";
  };
</script>

<style>
  form {
    width: 100%;
    display: flex;
  }

  .new-todo {
    width: 100%;
    flex: 1;
  }
</style>

<form on:submit|preventDefault={handleAddTodo}>
  <input bind:value={todoText} class="new-todo" placeholder="Add new todo" />
  <button type="submit">Add todo</button>
</form>
