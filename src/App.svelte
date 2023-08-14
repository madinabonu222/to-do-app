<script>
  import "./index.css";
  export let name;

  let todos = [
    // { done: false, text: "finish to-do app" },
    // { done: false, text: "prepare for driving test" },
    // { done: false, text: "washing dishes" },
  ];

  function add() {
    todos = todos.concat({ done: false, text: "" });
  }

  function clear() {
    todos = todos.filter((t) => !t.done);
  }
  $: remaining = todos.filter((t) => !t.done).length;
</script>

<header>
  <div class="box1">
    <h1>{name}!</h1>
    <h2>What do you need to do?</h2>
  </div>
</header>

<main class="box2">
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
  .box1 {
    background-color: rgb(102, 0, 255);
    opacity: 0.8;
    height: 100%;
    width: 100%;
    display: inline-block;
  }

  .box2 {
    background-color: darkgrey;
    opacity: 10;
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
