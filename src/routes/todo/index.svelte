<script lang="ts">
  import { supabase } from '$lib/supabaseClient';
  import { onMount } from "svelte";

  let newTask = '';
  let todos: any[] = [];

  let user = supabase.auth.user()

  onMount(() => {
    fetchItems();
  });

  const fetchItems = async () => {
    let { data, error } = await supabase
      .from("todos")
      .select("*")
      .order("inserted_at", { ascending: true });
    if (error) {
      alert(error.message);
    } else {
      if (data) todos = data;
    }
  };

  const add = async () => {
    let { data, error } = await supabase
        .from("todos")
        .insert({ task: newTask, user_id: user?.id })
        .single();
       
    if (error) {
      alert(error.message);
    } else {
      todos = [...todos, data];
      newTask = '';
    }
  };

  const remove = async (id: number) => {
    try {
      await supabase.from("todos").delete().eq("id", id);
      todos = todos.filter((t) => t.id != id);
    } catch (error) {
      if (error instanceof Error) alert(error.message);
    }
  };
</script>

<h1>To-Do List</h1>

<div>
  <input bind:value={newTask} type="text" placeholder="New item" />
  <button on:click={add}>Add</button>
</div>

<div>
  {#each todos as todo}
    <input bind:checked={todo.is_complete} type="checkbox">
    <span>{todo.task}</span>
    <button on:click={() => remove(todo.id)}>Remove</button>
    <br/>
  {/each} 
</div>
