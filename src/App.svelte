<script>
  let todos = [];
  let checkedTodos = [];
  let type = "all";
  function handleSubmit(e) {
    e.preventDefault();

    if (e.target[0].value.trim() !== "") {
      todos = [
        ...todos,
        {
          id: Math.ceil(Math.random() * 1000),
          checked: false,
          title: e.target[0].value,
        },
      ];
    } else {
      alert("Iltimos text kiriting!");
    }

    checkedTodos = [...todos];
    e.target[0].value = "";
  }

  function handleChecked(id) {
    todos[id].checked = true;
    checkedTodos = [...todos];
  }

  function handleRemove(id) {
    checkedTodos = todos.filter((_, i) => i !== id);
  }

  function handleChange(e) {
    type = e.target.value;
  }

  $: if (type === "done") {
    checkedTodos = todos.filter((item) => item.checked);
  } else if (type === "all") {
    checkedTodos = [...todos];
  } else if (type === "note-done") {
    checkedTodos = todos.filter((item) => !item.checked);
  }
</script>

<!-- svelte-ignore non-top-level-reactive-declaration -->
<div class="main">
  <h3>Todo List</h3>
  <form on:submit={handleSubmit}>
    <!-- svelte-ignore a11y-autofocus -->
    <input type="text" />
    <select on:change={handleChange}>
      <option value="all">All</option>
      <option value="done">Completed</option>
      <option value="note-done">Pending</option>
    </select>
  </form>
  <div class="todos">
    {#each checkedTodos as todo, index}
      <div class="todo">
        <p class:checked={todo.checked}>{todo.title}</p>
        {#if !todo.checked}
          <!-- svelte-ignore a11y-click-events-have-key-events -->
          <img
            src="./assets/check.png"
            alt="check"
            on:click={handleChecked.bind(this, index)}
          />
        {/if}
        <!-- svelte-ignore a11y-click-events-have-key-events -->
        <img
          src="./assets/remove.png"
          alt="remove"
          on:click={handleRemove.bind(this, index)}
        />
      </div>
    {/each}
  </div>
</div>

<style>
  .main {
    height: 100%;
    width: 100%;
    background: linear-gradient(112.58deg, #ff1e1e 0.37%, #9fce18 98.57%);
    display: flex;
    flex-direction: column;
    align-items: center;
    padding-top: 50px;
  }

  h3 {
    color: white;
    font-size: 24px;
    margin-bottom: 20px;
  }

  form {
    display: flex;
    align-items: center;
    gap: 20px;
    margin-bottom: 40px;
  }

  form input {
    width: 500px;
    outline: none;
    border: none;
  }

  form select {
    background-color: white;
  }

  .todos {
    display: flex;
    flex-direction: column;
    gap: 20px;
  }

  .todo {
    display: flex;
    align-items: center;
    justify-content: flex-start;
    gap: 20px;
  }

  .todo p {
    background-color: white;
    width: 450px;
    padding: 10px;
    border-radius: 4px;
  }
  .todo .checked {
    background: #f9d1d1;
    text-decoration-line: line-through;
  }

  .todo img {
    width: 40px;
    cursor: pointer;
  }
</style>
