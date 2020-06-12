<template>
  <thead>
    <th style="width: 30%">
      <a @click="changeSort('artist')">Artist
        <FontAwesomeIcon
          v-if="sortBy === 'artist' && sortDirection === 'asc'"
          icon="chevron-up"
        />
        <FontAwesomeIcon
          v-if="sortBy === 'artist' && sortDirection === 'desc'"
          icon="chevron-down"
        />
      </a>
    </th>
    <th style="width: 30%">
      <a @click="changeSort('title')">Title
        <FontAwesomeIcon
          v-if="sortBy === 'title' && sortDirection === 'asc'"
          icon="chevron-up"
        />
        <FontAwesomeIcon
          v-if="sortBy === 'title' && sortDirection === 'desc'"
          icon="chevron-down"
        />
      </a>
    </th>
    <th style="width: 20%">
      <a @click="changeSort('album')">Album
        <FontAwesomeIcon
          v-if="sortBy === 'album' && sortDirection === 'asc'"
          icon="chevron-up"
        />
        <FontAwesomeIcon
          v-if="sortBy === 'album' && sortDirection === 'desc'"
          icon="chevron-down"
        />
      </a>
    </th>
    <th style="width: 20%">
      <a @click="changeSort('genre')">Genre
        <FontAwesomeIcon
          v-if="sortBy === 'genre' && sortDirection ==='asc'"
          icon="chevron-up"
        />
        <FontAwesomeIcon
          v-if="sortBy === 'genre' && sortDirection === 'desc'"
          icon="chevron-down"
        />
      </a>
    </th>
  </thead>
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
      sortBy: '',
      sortDirection: 'asc'
    }
  },
  watch: {
    songs() {
      this.sortSongs();
    }
  },
  methods: {
    changeSort(column) {
      if (this.sortBy === column && this.sortDirection === 'asc') {
        this.sortDirection = 'desc';
      } else {
        this.sortDirection = 'asc';
      }
      this.sortBy = column;
      this.sortSongs();
    },
    sortSongs() {
      if (this.sortBy === "") {
        this.$emit('sort-songs', this.songs)
        return
      }

      let sortModifier = (this.sortDirection === 'asc') ? 1 : -1

      let sortedSongs = this.songs.slice().sort((a, b) => {
        let colA = a[this.sortBy].toUpperCase()
        let colB = b[this.sortBy].toUpperCase()

        if (colA < colB) {
          return -1 * sortModifier
        }
        if (colA > colB) {
          return sortModifier
        }

        return 0
      })

      this.$emit('sort-songs', sortedSongs)
    }
  }
}
</script>

<style>

</style>