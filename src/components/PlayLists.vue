<template>
  <nav class="panel">
    <p class="panel-heading">
      PlayLists
    </p>
    <template v-for="playList in playLists">
      <p
        :key="playList.slug"
        class="panel-block"
      >
        <span class="panel-icon"><FontAwesomeIcon icon="book" /></span>
        {{ playList.name }}
      </p>
    </template>
    <div class="panel-block">
      <form @submit.prevent="addPlaylist">
        <div class="filed has-addons">
          <input
            v-model="newPlayListName"
            type="text"
            class="input"
            placeholder="New Play List"
          >
          <p class="control">
            <button
              type="submit"
              class="button is-success"
            >
              <FontAwesomeIcon icon="plus" />
            </button>
          </p>
        </div>
      </form>
    </div>
  </nav>
</template>

<script>
import localforage from 'localforage';

export default {
  data() {
    return {
      newPlayListName: '',
      playLists: []
    };
  },
  methods: {
    addPlaylist() {
      this.playLists.push({
        name: this.newPlayListName,
        slug: this.slugify(this.newPlayListName),
        songs: []
      });
    },
    slugify (name) {
      return name.toString().toLowerCase().trim()
        .replace(/\s+/g, '-')         // Replace spaces with -
        .replace(/&/g, '-and-')       // Replace & with 'and'
        .replace(/[^\w-]+/g, '')      // Remove all non-word chars
        .replace(/--+/g, '-')         // Replace multiple - with single -
    }
  }
}
</script>

<style>

</style>
