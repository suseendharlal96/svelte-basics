<script>
  import { onMount } from "svelte";

  import PostForm from "../components/PostForm.svelte";

  const baseUrl = "https://ndb99xkpdk.execute-api.eu-west-2.amazonaws.com/dev";
  let posts = [];
  let selectedPost = {
    title: "",
    body: "",
    id: null,
  };
  onMount(async () => {
    const res = await fetch(baseUrl + "/posts");
    posts = await res.json();
  });

  const createPost = ({ detail: post }) => {
    const postIndex = posts.findIndex((p) => p.id === post.id);
    if (postIndex !== -1) {
      const a = [...posts];
      a[postIndex] = post;
      posts = a;
    } else {
      posts = [post, ...posts];
    }
    selectedPost = {
      title: "",
      body: "",
      id: null,
    };
  };
  const delPost = async (id) => {
    const res = await fetch(baseUrl + "/post/" + id, { method: "DELETE" });
    if (res) {
      posts = posts.filter((p) => p.id !== id);
    }
  };
  const editPost = (post) => {
    selectedPost = post;
  };
</script>

<div class="row">
  <div class="card s6">
    <PostForm on:postCreated={createPost} {selectedPost} />
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
            <button on:click={() => delPost(post.id)}>Delete</button>
          </div>
        </div>
      </div>
    {/each}
  {/if}
</div>
