<template>
  <nav class="panel">
    <a class="panel-block">
      <router-link to="/">All songs</router-link>
    </a>

    <p class="panel-heading">
      Playlists
    </p>

    <template v-for="playlist in playlists">
      <p
        :key="playlist.slug"
        class="panel-block"
      >
        <router-link :to="`playlist/${playlist.slug}`">
          <span class="panel-icon"><FontAwesomeIcon icon="book" /></span>
          {{ playlist.name }}
        </router-link>
      </p>
    </template>

    <div class="panel-block">
      <form @submit.prevent="addPlaylist">
        <div class="field has-addons ">
          <input
            v-model="newPlaylistName"
            type="text"
            class="input"
            placeholder="New Playlist"
          >
        </div>
        <p class="control">
          <button
            type="submit"
            class="button is-success"
          >
            <FontAwesomeIcon icon="plus" />
          </button>
        </p>
      </form>
    </div>
  </nav>
</template>

<script>
import localforage from 'localforage';

export default {
  data() {
    return {
      newPlaylistName: '',
      playlists: []
    }
  },
  watch: {
    playlists: {
      handler(playlists) {
        localforage.setItem('playlists', playlists);
      },
      deep: true
    }
  },
  created() {
    localforage.getItem('playlists')
      .then(data => {
        if (data !== null) {
          this.playlists = data;
        }
      })
  },
  methods: {
    addPlaylist() {
      this.playlists.push({
        name: this.newPlaylistName,
        slug: this.slugify(this.newPlaylistName),
        songs: []
      });

      this.newPlaylistName = '';
    },
    slugify(name) {
      return name.toString().toLowerCase().trim()
        .replace(/\s+/g, '-')         // Replace spaces with -
        .replace(/&/g, '-and-')       // Replace & with 'and'
        .replace(/[^\w-]+/g, '')      // Remove all non-word chars
        .replace(/--+/g, '-')         // Replace multiple - with single -
    }
  }
}
</script>

<style scoped>

</style>