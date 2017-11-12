<template>
  <main id="app">
    <section class="inputs">
    <label
      for="title-input">
      Title
    </label>
    <input
      id="title-input"
      type="text"
    />
    <label
      for="caption-input">
      Caption
    </label>
    <input
      id="caption-input"
      type="text"
    />
    <label
      class="file-label"
      for="file-input">
      Choose File
    </label>
    <input id="file-input" type="file">
    <button>Add to Album</button>
    </section>
    <section class="photo-album">
      Photos go here:
    </section>
  </main>

</template>

<script>
export default {
  name: "app",
  data() {
    return {
      msg: "Welcome to Your Vue.js App"
    };
  }
};
</script>

<style lang="scss">
$color-dark-purple: #3e2a35;
$color-purple: #5b4447;
$color-light-purple: #c2b2b4;
$color-border: #938284;
$color-white: #fff;
$color-black: #000;

body {
  font-size: 16px;
  font-family: 'Open Sans', sans-serif;
  margin: 0;
}

.inputs {
  display: block;
  background-color: $color-dark-purple;
  min-height: 30rem;
  padding: 2rem;
  label {
    color: $color-white;
  }

  input,
  label,
  button {
    display: block;
    margin-left: auto;
    margin-right: auto;
    text-align: center;
  }
}

#file-input {
  height: 0.1px;
  width: 0.1px;
  opacity: 0;
  overflow: hidden;
  position: absolute;
  z-index: -1;
}

.file-label {
  color: red;
}

.photo-album {
  background-color: $color-light-purple;
  min-height: 60rem;
}

</style>
