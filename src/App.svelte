<script>
  import { onMount } from "svelte"
    import TodoItem from "./TodoItem.svelte";
    import TodoForm from "./TodoForm.svelte";

  let todos = [];
  let newTodo = "";

  // körs när komponenten laddas 
  onMount(async () => {
    const res = await fetch("http://localhost:3001/todos")
    todos = await res.json()
  })

  async function addTodo()
  {
    if(!newTodo.trim()) return


    const res = await fetch("http://localhost:3001/todos", {
      method: "POST",
      headers: {
        "Content-Type": "application/json"
      },
      body: JSON.stringify({
        title: newTodo,
        completed: false
      })
    })

    const created = await res.json()
    todos = [...todos, created]
    newTodo = ""
  }


  async function toggleTodo(todo){
    const res = await fetch(`http://localhost:3001/todos/${todo.id}`,{
      method: "PATCH",
      headers: {
        "Content-Type": "application/json"
      }, 
      body: JSON.stringify({
        completed: !todo.completed
      })
    })
    const updated = await res.json()
    todos = todos.map(t => t.id === todo.id ? updated : t)
  }

  async function deleteTodo(id){
    await fetch(`http://localhost:3001/todos/${id}`, {
      method: "DELETE"
    })
    todos = todos.filter(t => t.id !== id)
  }

</script>

<main>
  <h1>Min Todo-lista</h1>


<TodoForm {addTodo} bind:newTodo />

  <!-- listan -->
  <ul>
    {#each todos as todo}
      <TodoItem {todo} toggle={toggleTodo} remove={deleteTodo} />
    {/each}
  </ul>
</main>


<style>
  ul{
    list-style: none;
    padding: 0;
  }

</style>