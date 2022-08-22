<script lang="ts">
  import { supabase } from '$lib/supabaseClient';

  let loading = false;
  let email = '';
  let password = '';

  const handleAuth = async (type: string) => {
    try {
      loading = true;
      const { user, error } = 
        type ==='login' 
          ? await supabase.auth.signIn({ email, password }) 
          : await supabase.auth.signUp({ email, password });

      if (error) throw error;
      else if (!user && !error) alert('A link has been sent to your email.');
    } catch (error) {
      if (error instanceof Error) alert(error.message);
    } finally {
      loading = false;
    }
  };
</script>

<form>
  <div>
    <h1>To-Do App</h1>
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
    <div>
      <input type="submit" value={loading ? "Loading" :
      "Log In"} disabled={loading} on:click={() => handleAuth('login')}/>

      <input type="submit" value={loading ? "Loading" :
      "Sign Up"} disabled={loading} on:click={() => handleAuth('signup')}/>
    </div>
  </div>
</form>