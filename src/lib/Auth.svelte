<script lang="ts">
  import { supabase } from '$lib/supabaseClient';

  let loading = false;
  let email = '';
  let password = '';
  let message = '';

  const handleAuth = async (type: string) => {
    loading = true;
    const { user, error } = 
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
  <h1>To-Do App</h1>
    <form>
    <p>Log in or sign up:</p>
    <div>
      <input
        type="email"
        placeholder="Your email"
        bind:value="{email}"
      />
      <input
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
      <input type="submit" value={loading ? "Loading" :
      "Log In"} disabled={loading} on:click={() => handleAuth('login')}/>

      <input type="submit" value={loading ? "Loading" :
      "Sign Up"} disabled={loading} on:click={() => handleAuth('signup')}/>
    </div>
  </form>
</div>