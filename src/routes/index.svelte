<script lang="ts">
  import { supabase } from '$lib/supabaseClient';
  import Auth from '$lib/Auth.svelte';
  import type { User } from '@supabase/supabase-js';
  import { onMount } from "svelte";

  let user: User | null;

  onMount(() => {
    const session = supabase.auth.session();
    user = session?.user ?? null;

    const { data: authListener } = supabase.auth.onAuthStateChange(
      (event, session) => {
        const currentUser = session?.user;
        user = currentUser ?? null;
      }
    );

    return () => {
      authListener?.unsubscribe();
    };
  });
</script>

<div>
  {#if user}
    <h1>Home</h1>
    <ul>
      <li><a href="/about">About</a></li>
      <li><a href="/todo">To-Do</a></li>
    </ul>
  {:else}
    <Auth />
  {/if}
</div>