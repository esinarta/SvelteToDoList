<script context="module" lang="ts">
  export async function load() {
    let { data, error } = await supabase
      .from("todos")
      .select("*")
      .order("inserted_at", { ascending: true });
    if (error) {
      return {
        error: new Error(error.message),
      }
    } else {
			return {
				props: {
          data
				}
			};
    }
  }
</script>

<script lang="ts">
  import { supabase } from '$lib/supabaseClient';
  import TodoItem from '$lib/TodoItem.svelte';
  import { onMount } from "svelte";
  export let data: any[];

  let newTask = '';
  let todos: any[] = [];

  let user = supabase.auth.user()

  onMount(() => {
    todos = data;
  });
  
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
    <TodoItem todo={todo} onDelete={() => remove(todo.id)} />
  {/each} 
</div>
