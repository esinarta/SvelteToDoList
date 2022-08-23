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
  <h1 class="text-3xl font-bold text-center my-4">To-Do App</h1>
  <form class="card w-96 bg-neutral shadow-xl mx-auto my-4">
    <div class="card-body items-center text-center">
      <p>Log in or sign up:</p>
      <div>
        <input
          class="input input-bordered w-full max-w-xs my-2"
          type="email"
          placeholder="Your email"
          bind:value="{email}"
        />
        <input
          class="input input-bordered w-full max-w-xs my-2"
          type="password"
          placeholder="Your password"
          bind:value="{password}"
        />
      </div>
      {#if message}
        <div class="my-2">
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
    </div>
  </form>
</div>