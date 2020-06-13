<template>
  <div class="columns">
    <div class="column is-3">
      <Playlists 
        :selected-playlist-slug="selectedPlaylistSlug"
        @set-playlist="setPlaylist"
      />
    </div>
    <div class="column is-9">
      <table class="table is-fullwidth is-striped is-hoverable is-narrow">
        <MusicSort
          :songs="songs"
          @sort-songs="sortSongs"
        />
        <PaginatedTableBody :items="sortedSongs">
          <template #customColumn="songList">
            <a
              class="button is-small"
              @click="deleteSong(songList.song)"
            >
              <FontAwesomeIcon icon="trash"></FontAwesomeIcon>
            </a>
          </template>
        </PaginatedTableBody>
      </table>
    </div>
  </div>
</template>

<script>
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
      selectedPlaylist: {},
      selectedPlaylistSlug: this.$route.params.slug,
      songs: [],
      sortedSongs: []
    }
  },
  methods: {
    sortSongs(data) {
      this.sortedSongs = data;
    },
    setPlaylist(playlist) {
      this.selectedPlaylist = playlist;
      this.songs = playlist.songs;
    },
    deleteSong(song) {
      this.selectedPlaylist.songs.splice(this.selectedPlaylist.songs.indexOf(song), 1);
    }
  },
  beforeRouteUpdate(to, from, next) {
    this.selectedPlaylist = to.params.slug;
    next();
  }

}
</script>

<style>

</style>
