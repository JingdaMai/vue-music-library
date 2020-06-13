<template>
  <p class="control has-icons-left">
    <input
      v-model="searchInput"
      class="input"
      type="text"
      placeholder="Search for song titles"
    >
    <span class="icon is-small is-left"><FontAwesomeIcon icon="search" /></span>
  </p>
</template>

<script>
export default {
  props: {
    songs: {
      type: Array,
      required: true
    }
  },
  data() {
    return {
      searchInput: []
    }
  },
  watch: {
    searchInput(searchTerm) {
      this.search(searchTerm)
    }
  },
  methods: {
    search(searchTerm) {
      if (searchTerm === '') {
        this.$emit('filtered-songs', this.songs);
        return;
      }

      const filteredSongs = this.songs.filter(song => {
        return (song.title.includes(searchTerm) || song.album.includes(searchTerm) ||
          song.artist.includes(searchTerm) || song.genre.includes(searchTerm));
      });
      this.$emit('filtered-songs', filteredSongs);
    },
  }
}
</script>

<style>

</style>