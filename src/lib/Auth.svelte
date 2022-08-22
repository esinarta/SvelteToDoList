<script lang="ts">
  import { supabase } from '$lib/supabaseClient';
  import "../app.css";

  let loading = false;
  let email = '';
  let password = '';
  let message = '';

  const handleAuth = async (type: string) => {
    loading = true;
    const { error } = 
      type ==='login' 
        ? await supabase.auth.signIn({ email, password }) 
        : await supabase.auth.signUp({ email, password });

    if (error) {
      message = error.message
    } else {
      message = 'A link has been sent to your email.';
    } 
    
    loading = false;
  }
</script>

<div>
  <h1 class="text-3xl font-bold">To-Do App</h1>
    <form>
    <p>Log in or sign up:</p>
    <div>
      <input
        class="input input-bordered w-full max-w-xs"
        type="email"
        placeholder="Your email"
        bind:value="{email}"
      />
      <input
        class="input input-bordered w-full max-w-xs"
        type="password"
        placeholder="Your password"
        bind:value="{password}"
      />
    </div>
    {#if message}
      <div>
        {message}
      </div>
    {/if}
    <div>
      <button 
        class="btn btn-primary" 
        type="submit" 
        disabled={loading} 
        on:click={() => handleAuth('login')}
      >
        Log In
      </button>
      <button 
        class="btn btn-primary" 
        type="submit" 
        disabled={loading} 
        on:click={() => handleAuth('signup')}
      >
        Sign Up
      </button>
    </div>
  </form>
</div>