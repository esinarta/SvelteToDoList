<script lang="ts">
  import { supabase } from '$lib/supabaseClient';
  import type { Todo } from '../models/Todo.type';

  export let todo: Todo;
  export let onDelete: () => void;

  let isComplete = todo.is_complete;

  const toggleComplete = async () => {
    const { data, error } = await supabase
      .from("todos")
      .update({ is_complete: !isComplete })
      .eq("id", todo.id)
      .single();

    if (error) {
      alert(error.message);
    } else {
      isComplete = data.is_complete;
    }
  };
</script>

<div class="grid grid-cols-8 gap-2 w-80 my-2">
  <input 
    class="checkbox checkbox-primary col-span-1" 
    type="checkbox" 
    on:change={toggleComplete}
    bind:checked={isComplete}
  />
  <span class="col-span-5">{todo.task}</span>
  <button 
    class="btn btn-error btn-sm col-span-2" 
    on:click={onDelete}
  >
    Remove
  </button>
</div>