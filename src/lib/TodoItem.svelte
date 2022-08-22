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

<div>
  <input on:change={toggleComplete} bind:checked={isComplete} type="checkbox">
  <span>{todo.task}</span>
  <button on:click={onDelete}>Remove</button>
</div>