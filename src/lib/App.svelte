<script>
  import { user } from "$lib/stores/sessionStore";
  import { supabase } from "$lib/db/supabaseClient";
  import Auth from "$lib/Auth.svelte";
  import Profile from "$lib/Profile.svelte";

  user.set(supabase.auth.user());

  supabase.auth.onAuthStateChange((_, session) => {
    user.set(session.user);
  });
</script>

<main>
  <div class="container">
    <!-- <details class="debug">
      <summary>Debug:</summary>
      <p>$user: {$user}</p>
      <p>$user?.role: {$user?.role}</p>
      <p>$user?.aud: {$user?.aud}</p>
      <pre>{JSON.stringify($user, null, 2)}</pre>
    </details>
    <hr /> -->

    {#if $user?.role}
      <Profile />
    {:else}
      <Auth />
    {/if}
  </div>
</main>

<style>
  main {
    text-align: center;
    padding: 1rem;
    margin: 0 auto;
  }

  .debug {
    text-align: left;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
