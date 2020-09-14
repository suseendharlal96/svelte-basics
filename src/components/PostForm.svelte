<script>
  const onSubmit = () => {
    console.log("sent");
  };
</script>

<form on:submit|preventDefault={onSubmit}>
  <div class="input-field">
    <label for="title">Title</label>
    <input type="text" />
  </div>
</form>
