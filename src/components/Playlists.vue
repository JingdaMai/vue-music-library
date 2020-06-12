<template>
  <nav class="panel">
    <a class="panel-block">
      <router-link to="/">All songs</router-link>
    </a>

    <p class="panel-heading">
      Playlists
    </p>

    <template v-for="(playlist, index) in playlists">
      <p
        :key="playlist.slug"
        class="panel-block playlist-item"
      >
        <router-link
          :to="`playlist/${playlist.slug}`"
          class="panel-block"
        >
          <span class="panel-icon"><FontAwesomeIcon icon="book" /></span>
          {{ playlist.name }}
        </router-link>
        
        <template v-if="addingEnabled">
          <a
            v-if="!playlist.adding"
            title="Add songs"
            @click="addSongs(index)"
          >
            <FontAwesomeIcon icon="plus" />
          </a>

          <a
            v-if="playlist.adding"
            title="Disable adding songs"
            @click="addSongs(index)"
          >
            <FontAwesomeIcon icon="check-square" />
          </a>
        </template>
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
  props: {
    addingEnabled: {
      type: Boolean,
      default: false,
      required: false
    },
    selectedPlaylistSlug: {
      type: String,
      default: '',
      required: false
    }
  },
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
    },
    selectedPlaylistSlug(slug) {
      this.$emit('set-playlist', this.playlists.find(pl => pl.slug === slug));
    }
  },
  created() {
    localforage.getItem('playlists')
      .then(data => {
        if (data !== null) {
          this.playlists = data;
          this.playlists.forEach((pl, index) => {
            this.playlists[index].adding = false;
          })
        }
      })
      .then(() => {
        if (this.selectedPlaylistSlug) {
          this.$emit('set-playlist', this.playlists.find(pl => pl.slug === this.selectedPlaylistSlug));
        }
      })
  },
  methods: {
    addPlaylist() {
      this.playlists.push({
        name: this.newPlaylistName,
        slug: this.slugify(this.newPlaylistName),
        adding: false,
        songs: []
      });

      this.newPlaylistName = '';
    },
    addSongs(index) {
      this.playlists[index].adding = !this.playlists[index].adding;
      this.$emit('set-active-playlists', this.playlists.filter(pl => pl.adding===true));
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
nav {
  position: fixed;
  width: inherit;
}
.playlist-item {
  justify-content: space-between;
}
</style>