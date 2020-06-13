<template>
  <div class="columns">
    <div class="column is-3">
      <Playlists
        adding-enabled
        @set-active-playlists="setActivePlaylists"
      />
    </div>
    <div class="column is-9">
      <MusicSearch :songs="songs" @filtered-songs="filterSongs">

      </MusicSearch>
      <table class="table is-fullwidth is-striped is-hoverable is-narrow">
        <MusicSort
          :songs="filteredSongs"
          @sort-songs="sortSongs"
        />
        <PaginatedTableBody :items="sortedSongs">
          <template 
            v-if="activePlaylists.length > 0" 
            #addTitle="songList"
          >
            <a @click="addSong(songList.song, $event.target)">
              <FontAwesomeIcon icon="plus" />
            </a>
          </template>
        </PaginatedTableBody>
      </table>
    </div>
  </div>
</template>

<script>
import MusicData from '../assets/list.json';

import MusicSearch from "./MusicSearch";
import MusicSort from "./MusicSort";
import PaginatedTableBody from "./PaginatedTableBody";
import Playlists from "./Playlists";

export default {
  components: {
    MusicSearch,
    MusicSort,
    PaginatedTableBody,
    Playlists
  },
  data() {
    return {
      songs: MusicData,
      sortedSongs: MusicData,
      filteredSongs: MusicData,
      activePlaylists: []
    }
  },
  methods: {
    filterSongs(data) {
      this.filteredSongs = data;
    },
    sortSongs(data) {
      this.sortedSongs = data;
    },
    setActivePlaylists(playlists) {
      this.activePlaylists = playlists;
    },
    addSong(song, e) {
      e.closest('tr').classList.add('flash');
      setTimeout(() => e.closest('tr').classList.remove('flash'), 1000);
      this.activePlaylists.forEach((pl) => {
        pl.songs.push(song);
      })
    }
  }
}
</script>

<style>

</style>
