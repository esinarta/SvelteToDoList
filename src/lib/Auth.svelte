<script lang="ts">
  import { supabase } from './supabaseClient';

  let loading: boolean = false;
  let email: string = '';
  let password: string = '';

  const handleAuth = async (type: string) => {
    try {
      loading = true;
      const { user, error } = 
        type ==='login' 
          ? await supabase.auth.signIn({ email, password }) 
          : await supabase.auth.signUp({ email, password });

      if (error) throw error;
      else if (!user && !error) alert('A link has been sent to your email.');
    } catch (error: any) {
      alert(error.message);
    } finally {
      loading = false;
    }
  };
</script>

<form>
  <div>
    <h1>Supabase + Svelte</h1>
    <p>Sign in via magic link with your email below</p>
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