<script>
  import { onMount } from "svelte";
  import "./index.css";
  export let name;

  let todos = [
    // { done: false, text: "finish to-do app" },
    // { done: false, text: "prepare for driving test" },
    // { done: false, text: "washing dishes" },
  ];
  let searchText = "";
  // Load todos from localStorage on component mount
  onMount(() => {
    const storedTodos = localStorage.getItem("todos");
    if (storedTodos) {
      todos = JSON.parse(storedTodos);
    }
  });

  function add() {
    todos = todos.concat({ done: false, text: "" });
  }

  function clear() {
    todos = todos.filter((t) => !t.done);
  }
  function filter(todo) {
    todos = todos.filter((t) => t !== todo);
  }
  function saveTodos() {
    localStorage.setItem("todos", JSON.stringify(todos));
  }

  $: remaining = todos.filter((t) => !t.done).length;

  // Save todos to localStorage whenever todos or searchText changes
  $: {
    saveTodos();
  }
</script>

<header>
  <div class="header">
    <h1>{name}!</h1>
    <h2>What do you need to do?</h2>
  </div>
</header>

<main class="main">
  {#each todos as todo}
    <div class:done={todo.done}>
      <input type="checkbox" bind:checked={todo.done} />

      <input placeholder="What needs to be done?" bind:value={todo.text} />
    </div>
  {/each}
  <div id="search-input-cont">
    <input
      type="text"
      id="search-field"
      placeholder="Filter todos"
      autocapitalize="off"
      bind:value={todos}
      on:input
    />
  </div>
  <p>{remaining} remaining</p>

  <button on:click={add}> Add new </button>

  <button on:click={clear}> Clear completed </button>
</main>

<style>
  .header {
    background-color: rgb(102, 0, 255);
    opacity: 0.8;
    height: 100%;
    width: 100%;
    display: inline-block;
  }
  #search-input-cont {
    width: 14%;
    display: flex;
    align-items: center;
    font-family: "Comic Sans MS", cursive;
    margin: 0 0 0 1px;
  }

  #search-field {
    width: 100%;
    font-size: 1.1rem;
    border: 3px solid rgba(48, 17, 187, 0.635);
    border-radius: 5px;
    padding: 5px;
    margin: 0 0px 0;
  }

  .main {
    background-color: darkgrey;
    opacity: 10;
    /* display: block; */
  }

  header {
    display: block;
    text-align: center;
    margin: 0;
    padding: 0;
  }

  h1 {
    color: white;
    text-transform: uppercase;
    font-family: "Comic Sans MS", cursive;
    font-size: 3em;
    font-weight: 500;
  }
  h2 {
    color: white;
    text-transform: lowercase;
    font-family: "Comic Sans MS", cursive;
    font-size: 2em;
    font-weight: 400;
  }
  h2::first-letter {
    text-transform: uppercase;
  }
  p {
    color: rgb(99, 63, 140);
    text-transform: lowercase;
    text-align: left;
    font-family: "Comic Sans MS", cursive;
    font-size: 1.5em;
    font-weight: 300;
  }

  button {
    color: "red";
    text-align: center;
    font-family: "Comic Sans MS", cursive;
  }
</style>
