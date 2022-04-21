<script>
  import { supabase } from "$lib/db/supabaseClient";

  let loading = false;
  let requested = false;
  let email;
  let errorMsg = null;

  const handleLogin = async () => {
    errorMsg = null;

    try {
      loading = true;
      const { error } = await supabase.auth.signIn({ email });
      if (error) throw error;
      console.log("Check your email for the login link!");
      requested = true;
    } catch (error) {
      errorMsg = error.error_description || error.message;
      console.log("ERROR - Login", errorMsg);
    } finally {
      loading = false;
    }
  };
</script>

<form class="row flex flex-center" on:submit|preventDefault={handleLogin}>
  <div class="col-12 form-widget">
    <h2 class="header">Supabase Auth Demo</h2>
    <p class="description">Sign in via magic link with your email below</p>
    <div>
      <input
        class="inputField"
        type="email"
        placeholder="Your email"
        bind:value={email} />
    </div>
    <div>
      {#if !requested}
        <input
          type="submit"
          class="button block"
          value={loading ? "Loading" : "Send magic link"}
          disabled={loading || requested} />
      {/if}
      {#if requested}
        <p>Check your email for the login link!</p>
      {/if}
      {#if errorMsg}
        <p class="error">{errorMsg}</p>
      {/if}
    </div>
  </div>
</form>

<style>
  .error {
    color: red;
  }
</style>
