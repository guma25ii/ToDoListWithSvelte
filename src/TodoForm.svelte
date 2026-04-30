<script>
  export let addTodo
  export let newTodo
</script>

<div>
  <input
    bind:value={newTodo}
    placeholder="Ny todo"
  />

  <button on:click={addTodo}>
    Lägg till
  </button>
</div>