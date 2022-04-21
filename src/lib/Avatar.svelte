<script>
  import { createEventDispatcher } from "svelte";
  import { supabase } from "$lib/db/supabaseClient";

  export let path;
  export let size = "10rem";

  let uploading = false;
  let src;
  let files;

  const dispatch = createEventDispatcher();

  async function downloadImage() {
    try {
      const { data, error } = await supabase.storage
        .from("avatars")
        .download(path);
      if (error) throw error;

      src = URL.createObjectURL(data);
    } catch (error) {
      console.error("Error downloading image: ", error.message);
    }
  }

  async function uploadAvatar() {
    try {
      uploading = true;

      if (!files || files.length === 0) {
        throw new Error("You must select an image to upload.");
      }

      const file = files[0];
      const fileExt = file.name.split(".").pop();
      const fileName = `${Math.random()}.${fileExt}`;
      const filePath = `${fileName}`;

      let { error: uploadError } = await supabase.storage
        .from("avatars")
        .upload(filePath, file);

      if (uploadError) throw uploadError;

      path = filePath;
      dispatch("upload");
    } catch (error) {
      console.log(error.message);
    } finally {
      uploading = false;
    }
  }
</script>

<div class="center">
  {#if path}
    <img
      use:downloadImage
      {src}
      alt="Avatar"
      class="avatar image"
      style="height: {size}; width: {size};" />
  {:else}
    <div class="avatar no-image" style="height: {size}; width: {size};" />
  {/if}

  <div class="btn" style="width: {size};">
    <label class="button primary block" for="single">
      {uploading ? "Uploading ..." : path ? "Replace" : "Upload"}
    </label>
    <input
      style="visibility: hidden; position:absolute;"
      type="file"
      id="single"
      accept="image/*"
      bind:files
      on:change={uploadAvatar}
      disabled={uploading} />
  </div>
</div>

<style>
  .btn {
    margin-top: 1rem;
  }
</style>
