<script context="module" lang="ts">
  export async function load() {
    const { data, error } = await supabase
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
  import type { Todo } from '../../models/Todo.type';

  export let data: Todo[];

  let newTask = '';
  let todos: Todo[] = [];

  const user = supabase.auth.user()

  onMount(() => {
    todos = data;
  });
  
  const add = async () => {
    const { data, error } = await supabase
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
    const { error } = await supabase.from("todos").delete().eq("id", id);
    todos = todos.filter((t) => t.id != id);

    if (error) {
      alert(error.message);
    }
  };
</script>

<div>
  <h1 class="text-3xl font-bold">To-Do List</h1>

  <div class="form-control">
    <div class="input-group">
      <input
        class="input input-bordered w-full max-w-xs"
        type="text"
        placeholder="New Item"
      />
      <button class="btn" on:click={add}>Add</button>
    </div>
  </div>
  
  <div>
    {#each todos as todo}
      <TodoItem todo={todo} onDelete={() => remove(todo.id)} />
    {/each} 
  </div>
</div>
