<script lang="ts">
  import { supabase } from '$lib/supabaseClient';
  import Auth from '$lib/Auth.svelte';
  import type { User } from '@supabase/supabase-js';
  import { onMount } from "svelte";
  import "../app.css";

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
    <h1 class="text-3xl font-bold">Home</h1>
    <ul>
      <li><a class="link link-secondary" href="/about">About</a></li>
      <li><a class="link link-secondary" href="/todo">To-Do</a></li>
    </ul>
    <button 
      class="btn btn-primary" 
      type="submit" 
      on:click={async () => {
        const { error } = await supabase.auth.signOut();
        if (error) alert(error.message);
      }}
    >
      Log Out
    </button>
  {:else}
    <Auth />
  {/if}
</div>