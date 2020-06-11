<template>
  <table class="table is-fullwidth is-striped is-hoverable is-narrow">
    <MusicSort
      :songs="songs"
      @sorted-songs="sortSongs"
    />
    <PaginatedTableBody :items="songs" />
  </table>
</template>

<script>
import MusicData from '../assets/list.json';
import MusicSort from "./MusicSort";
import PaginatedTableBody from "./PaginatedTableBody";

export default {
  components: {
    MusicSort,
    PaginatedTableBody
  },
  data() {
    return {
      songs: MusicData
    }
  },
  methods: {
    sortSongs(data) {
      this.songs = data;
    }
  }
}
</script>

<style>

</style>
