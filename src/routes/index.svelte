<script lang="ts">
  import { user } from '../lib/sessionStore';
  import { supabase } from '../lib/supabaseClient';
  import Auth from '../lib/Auth.svelte';

  user.set(!!supabase.auth.user());

  supabase.auth.onAuthStateChange((_, session) => {
    user.set(!!session?.user);
  });
</script>

<div>
  {#if $user}
  <h1>Home</h1>
  <ul>
    <li><a href="/about">About</a></li>
    <li><a href="/todo">To-Do</a></li>
  </ul>
  {:else}
  <Auth />
  {/if}
</div>