<script lang="ts">
  import { supabase } from '$lib/supabaseClient';

  export let todo: any;
  export let onDelete: () => void;

  let isComplete = todo.is_complete;

  const toggleComplete = async () => {
    try {
      const { data, error } = await supabase
        .from("todos")
        .update({ is_complete: !isComplete })
        .eq("id", todo.id)
        .single();
      if (error) {
        throw error;
      }
      isComplete = data.is_complete;
    } catch (error) {
      if (error instanceof Error) alert(error.message);
    }
  };
</script>

<div>
  <input on:change={toggleComplete} bind:checked={isComplete} type="checkbox">
  <span>{todo.task}</span>
  <button on:click={onDelete}>Remove</button>
</div>