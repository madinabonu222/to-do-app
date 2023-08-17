<script>
  import { onMount, onDestroy } from "svelte";
  import "./index.css";
  export let name;
  let flag = false;
  let todos = [
    // { done: false, text: "finish to-do app" },
    // { done: false, text: "prepare for driving test" },
    // { done: false, text: "washing dishes" },
  ];
  let searchText = "";
  // Load todos from localStorage on component mount
  onMount(() => {
    const storedTodos = localStorage.getItem("todos");
    console.log("this is from onMount, storedTodos", storedTodos);
    if (storedTodos) {
      todos = JSON.parse(storedTodos);
    }
    flag = true;
  });

  onDestroy(() => {
    const storedTodos = localStorage.getItem("todos");
    console.log("this is onDestroy", onDestroy);
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
    if (flag === true) {
      console.log("this is another one", todos);
      saveTodos();
    }
  }
</script>

<header>
  <div class="header">
    <h1>{name}!</h1>
    <h2>What do you need to do?</h2>
  </div>
</header>

<main class="main">
  <div id="search-input-cont">
    <input
      type="text"
      id="search-field"
      placeholder="Filter todos"
      autocapitalize="off"
      on:input
    />
  </div>

  {#each todos as todo}
    <div class:done={todo.done}>
      <input type="checkbox" bind:checked={todo.done} />

      <input placeholder="What needs to be done?" bind:value={todo.text} />
    </div>
  {/each}

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
    width: 200px;
    font-size: 1rem;
    border: 2px solid rgba(48, 17, 187, 0.635);
    border-radius: 5px;
    padding: 5px;
    margin: 15px;
    box-shadow: 5px 5px blueviolet;
  }

  .main {
    margin: 0px;
    border: 1px solid white;
    padding: 50px;
    background-color: #f2f3f4;
    opacity: 10;
    min-height: 100%;

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
    font-size: 1.1em;
    font-weight: 300;
  }

  button {
    /* color: black; */
    text-align: center;
    font-family: "Comic Sans MS", cursive;
  }
</style>
