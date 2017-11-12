<template>
  <main id="app">
    <section class="inputs-container">
      <article class="inputs">
        <h1 class="logo-banner">Foto <span class="logo"></span> Finder</h1>
        <div class="input-group">
          <label
            for="title-input">
            Title
          </label>
          <input
            v-model="title"
            id="title-input"
            type="text"
          />
        </div>
        <div class="input-group">
          <label
            for="caption-input">
            Caption
          </label>
          <input
            v-model="caption"
            id="caption-input"
            type="text"
          />
        </div>
        <div class="button-container">
          <input
            id="file-input"
            ref="image"
            type="file"
            @change="onFileChange"
          >
          <label
            class="file-label"
            for="file-input">
            Choose File
          </label>
          <button
            @click="validateFoto"
            :disabled="isDisabled"
          >
            Add to Album
          </button>
        </div>
        <h2 v-if="error" class="error">{{ error }}</h2>
      </article>
    </section>
    <section class="foto-album">
      <h2 v-if="noPhotos">Please use the form above to add a photo to the album!</h2>
      <div
        v-for="foto in fotos"
        :class="[{favorited: foto.favorite}, 'foto-card']"
        :key="foto.id"
      >
        <h3>{{ foto.title }}</h3>
        <img :src="foto.path" alt="User Uploaded Foto" />
        <p>{{ foto.caption }}</p>
        <div class="card-footer">
          <button
            @click="deleteFoto(foto.id)"
            class="delete-button"
          />
          <button
            class="favorite-button"
            @click="favorite(foto.id)"
          />
        </div>
      </div>
    </section>
  </main>

</template>

<script>
const EMPTY_TITLE = 'Please add a title';
const EMPTY_CAPTION = 'Please add a caption';
const EMPTY_FOTO = 'Please select a photo';

export default {
  name: 'app',
  data() {
    return {
      fotos: [],
      selectedFoto: '',
      title: '',
      caption: '',
      error: '',
    };
  },
  computed: {
    isDisabled() {
      return !this.title || !this.caption || !this.selectedFoto;
    },
    noPhotos() {
      return !this.fotos.length;
    },
  },
  methods: {
    clearInputs() {
      this.title = '';
      this.caption = '';
      this.selectedFoto = '';
    },
    onFileChange() {
      this.selectedFoto = `src/assets/${this.$refs.image.files[0].name}`;
    },
    validateFoto() {
      if (!this.title) {
        this.error = EMPTY_TITLE;
      } else if (!this.caption) {
        this.error = EMPTY_CAPTION;
      } else if (!this.selectedFoto) {
        this.error = EMPTY_FOTO;
      } else {
        this.addFoto();
        this.clearInputs();
      }
    },
    addFoto() {
      const { title, caption, selectedFoto: path } = this;

      const foto = {
        title,
        caption,
        path,
        favorite: false,
        id: Date.now(),
      };

      this.fotos.push(foto);
    },
    favorite(fotoId) {
      const id = parseInt(fotoId);

      return this.fotos.find(foto => {
        if (foto.id === id) {
          foto.favorite = !foto.favorite;
        }
      });
    },
    deleteFoto(fotoId) {
      const id = parseInt(fotoId);

      this.fotos = this.fotos.filter(foto => {
        return foto.id !== id;
      });
    },
  },
};
</script>

<style lang="scss">
$color-dark-purple: #3e2a35;
$color-purple: #5b4447;
$color-light-purple: #c2b2b4;
$color-border: #938284;
$color-white: #fff;
$color-light-white: #ddd;
$color-black: #000;

html {
  box-sizing: border-box;
}

*,
*:before,
*:after {
  box-sizing: inherit;
}

body {
  font-family: 'Open Sans', sans-serif;
  margin: 0;
}

button:disabled {
  pointer-events: none;
  cursor: inherit;
  background-color: #ddd !important;
  color: gray !important;
}

.inputs-container {
  display: block;
  background-color: $color-dark-purple;
  min-height: 30rem;
  padding: 2rem;

  @media only screen and (max-width: 640px) {
    padding: 0.5rem;
  }

  label {
    color: $color-white;
  }

  input,
  label,
  button {
    display: block;
    margin-left: auto;
    margin-right: auto;
  }

  .logo-banner {
    color: $color-white;
    font-size: 3rem;
    font-weight: 800;
    margin-top: 0;
    margin-bottom: 1rem;
    text-align: center;
    text-transform: uppercase;

    .logo {
      background: url('./assets/foto-finder-logo.svg') center no-repeat;
      content: '';
      display: inline-block;
      height: 2.75rem;
      width: 4rem;
    }

    @media only screen and (max-width: 640px) {
      font-size: 2rem;

      .logo {
        height: 1.75rem;
        width: 3rem;
      }
    }
  }

  #title-input,
  #caption-input {
    width: 100%;
    height: 2.5rem;
    padding: 0.25rem;
    font-size: 1.25rem;
  }

  .input-group {
    display: block;
    width: 100%;
    margin-left: auto;
    margin-right: auto;
    margin-bottom: 2rem;

    label {
      margin-bottom: 0.5rem;
    }
  }

  .inputs {
    width: 60%;
    margin-left: auto;
    margin-right: auto;

    @media only screen and (max-width: 640px) {
      width: 90%;
    }

    @media only screen and (min-width: 1100px) {
      width: 40%;
    }
  }

  input[type='file'] {
    height: 0.1px;
    width: 0.1px;
    opacity: 0;
    overflow: hidden;
    position: absolute;
    z-index: -1;
  }

  .button-container {
    margin-left: auto;
    margin-right: auto;
    display: block;
    min-height: 3rem;
    width: 100%;

    button,
    .file-label {
      background: $color-light-purple;
      border: none;
      color: $color-white;
      cursor: pointer;
      font-size: 1rem;
      width: 8rem;
      text-align: center;

      @media only screen and (max-width: 768px) {
        float: none !important;
        display: block;
        margin-bottom: 2rem;
        width: 100%;
      }
    }
    .file-label {
      padding: 0.5rem;
      float: left;
    }
    button {
      padding: 9px;
      padding-top: 10px;
      float: right;
    }
  }

  .error {
    font-weight: 800;
    color: $color-white;
    text-align: center;
  }
}

.foto-album {
  background-color: $color-light-purple;
  min-height: 40rem;
  display: flex;
  flex-flow: row wrap;
  padding: 2rem;

  h3,
  p {
    padding: 0 1rem;
  }

  .foto-card {
    background: $color-white;
    border: 1px solid $color-border;
    height: 25rem;
    width: 15rem;
    position: relative;
    margin: 1rem;

    img {
      display: block;
      max-width: 100%;
      max-height: 12rem;
      margin-left: auto;
      margin-right: auto;
      margin-bottom: 1rem;
    }

    p {
      position: absolute;
      bottom: 4rem;
    }

    .card-footer {
      background-color: $color-dark-purple;
      display: flex;
      justify-content: space-between;
      position: absolute;
      bottom: 0;
      padding: 1rem;
      width: 100%;

      button {
        cursor: pointer;
        padding: 0;
        border: none;
        background: transparent;
        content: '';
        display: block;
        height: 36px;
        width: 36px;
      }
      .delete-button {
        background: url('assets/delete.svg') center no-repeat;
        &:hover,
        &:focus {
          background: url('assets/delete-active.svg') center no-repeat;
        }
      }
      .favorite-button {
        background: url('assets/favorite.svg') center no-repeat;
      }
    }

    &.favorited {
      background-color: $color-light-white;
      .favorite-button {
        background: url('assets/favorite-active.svg') center no-repeat;
      }
    }
  }
}
</style>
