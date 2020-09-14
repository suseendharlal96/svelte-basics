<script>
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher();
  export let selectedPost;
  $: title = selectedPost.title;
  $: body = selectedPost.body;
  let loading = false;
  const baseUrl = "https://ndb99xkpdk.execute-api.eu-west-2.amazonaws.com/dev";
  const onSubmit = async () => {
    loading = true;
    console.log("sent");
    if (title.trim === "" || title.body === "") {
      alert("all fields are required");
    }
    const newPost = { title, body };
    let url;
    let method;
    if (selectedPost.id) {
      url = baseUrl + "/post/" + selectedPost.id;
      method = "PUT";
    } else {
      url = baseUrl + "/post/";
      method = "POST";
    }
    const res = await fetch(url, {
      method: method,
      body: JSON.stringify(newPost),
    });
    loading = false;
    title = "";
    body = "";
    const post = await res.json();
    dispatch("postCreated", post);
  };
</script>

{#if !loading}
  <form on:submit|preventDefault={onSubmit}>
    <div class="input-field">
      <label for="title">Title</label>
      <input type="text" name="title" bind:value={selectedPost.title} />
    </div>
    <div class="input-field">
      <label for="post">Post</label>
      <input type="text" name="body" bind:value={selectedPost.body} />
    </div>
    <button
      class="waves-effect waves-light btn"
      type="submit">{selectedPost && selectedPost.id ? 'Update' : 'Add'}</button>
  </form>
{:else}
  <div class="progress">
    <div class="indeterminate" />
  </div>
{/if}
