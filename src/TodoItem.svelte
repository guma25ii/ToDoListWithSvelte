<script>
  export let todo
  export let toggle
  export let remove
</script>

<li>
  <!-- checkbox -->
  <input
    type="checkbox"
    checked={todo.completed}
    on:change={() => toggle(todo)}
  />

  <!-- text -->
  <span>
    {todo.title}
  </span>

  <!-- delete -->
  <button on:click={() => remove(todo.id)}>
    Delete
  </button>
</li>

<style>
    li {
    display: flex;
    gap: 10px;
    margin-bottom: 10px;
  }
</style>