<script>
  import { onMount } from "svelte";

  import PostForm from "../components/PostForm.svelte";
  
  const baseUrl = "https://ndb99xkpdk.execute-api.eu-west-2.amazonaws.com/dev";
  let posts = [];

  onMount(async () => {
    const res = await fetch(baseUrl + "/posts");
    posts = await res.json();
    console.log(posts);
  });

  const editPost = (post) => {
    console.log(post);
  };
  const delPost = (post) => {
    console.log(post);
  };
</script>

<div class="row">
  <div class="card s6">
    <PostForm />
  </div>
</div>
<div class="row">
  {#if posts.length === 0}
    <h3>Loading..</h3>
  {:else}
    {#each posts as post}
      <div class="col">
        <div class="card s6">
          <div class="card-content">
            <p class="card-title">{post.title}</p>
            <p>{post.title}</p>
            <p>{post.createdAt}</p>
            <p>{post.body}</p>
          </div>
          <div class="card-action">
            <button on:click={() => editPost(post)}>Edit</button>
            <button on:click={() => delPost(post)}>Delete</button>
          </div>
        </div>
      </div>
    {/each}
  {/if}
</div>
