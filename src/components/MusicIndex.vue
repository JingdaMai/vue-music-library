<template>
  <div class="columns">
    <div class="column is-3">
      <Playlists />
    </div>
    <div class="column is-9">
      <table class="table is-fullwidth is-striped is-hoverable is-narrow">
        <MusicSort
          :songs="songs"
          @sort-songs="sortSongs"
        />
        <PaginatedTableBody :items="sortedSongs" />
      </table>
    </div>
  </div>
</template>

<script>
import MusicData from '../assets/list.json';

import MusicSort from "./MusicSort";
import PaginatedTableBody from "./PaginatedTableBody";
import Playlists from "./Playlists";

export default {
  components: {
    MusicSort,
    PaginatedTableBody,
    Playlists
  },
  data() {
    return {
      songs: MusicData,
      sortedSongs: MusicData
    }
  },
  methods: {
    sortSongs(data) {
      this.sortedSongs = data;
    }
  }
}
</script>

<style>

</style>
