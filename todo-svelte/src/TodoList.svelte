<script>
  import TodoItem from "./TodoItem.svelte";
  import { onMount } from "svelte";

  let todos = [];
  let newTodo = "";
  let ctr = 0;
  let input;

  onMount(function() {
    const json = localStorage.getItem("catch-todoitems");
    if (json) {
      todos = JSON.parse(json);
    }
  });

  function addTodoItem() {
    if (newTodo && newTodo.length > 0) {
      todos = [...todos, { title: newTodo, id: ++ctr }];
      save();
      newTodo = "";
    }
    input.focus();
  }

  function onRemoved(event) {
    todos = todos.filter(t => t.id !== event.detail.id);
    save();
    input.focus();
  }

  function save() {
    localStorage.setItem("catch-todoitems", JSON.stringify(todos));
  }
</script>

<style>
  .list {
    display: flex;
    flex-direction: column;
    max-width: 300px;
    margin: 0 auto;
  }

  .list-header {
    text-align: center;
  }
  .list-inputs {
    display: flex;
  }
  .list-inputs-input {
    width: 100%;
  }

  .list-todoitems-placeholder-container {
    text-align: center;
    color: #ccc;
    border: 1px dashed cornflowerblue;
    padding: 10px;
    margin-top: 5px;
  }
</style>

<div class="list">
  <h3 class="list-header">Todo List</h3>
  <div class="list-inputs">
    <input class="list-inputs-input" bind:value={newTodo} bind:this={input} />
    <button class="list-inputs-button" on:click={addTodoItem}>+</button>
  </div>
  {#if todos && todos.length > 0}
    <div class="list-todoitems">
      {#each todos as { title, id }, i}
        <TodoItem {title} {id} on:removed={onRemoved} />
      {/each}
    </div>
  {:else}
    <div class="list-todoitems-placeholder-container">
      <div>Add items to see them here!</div>
    </div>
  {/if}
</div>
