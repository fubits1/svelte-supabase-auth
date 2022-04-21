# Svelte + Supabase Auth Demo

[![Netlify Status](https://api.netlify.com/api/v1/badges/75d854f6-177c-420d-8139-c137287936c1/deploy-status)](https://app.netlify.com/sites/sveltekit-supabase-auth/deploys)

Adapted from: https://supabase.com/docs/guides/with-svelte for https://kit.svelte.dev

> since `svelte-kit` uses `vite` we need to prefix the environment variables with `VITE_` (locally in `.env` and globally in your deployment environment [i.e. Netlify])

Set up the Supabase project as described in https://supabase.com/docs/guides/with-svelte (also see `utils/db/create-auth-table.sql`) and add the following to your local (`.gitignore`-ed) `.env` file:

```
VITE_SVELTE_APP_SUPABASE_URL=<your-project-url>
VITE_SVELTE_APP_SUPABASE_ANON_KEY=<your-public-api-key>
```

> Note: The build directory has been set to `build` to comply with the Netlify default.
