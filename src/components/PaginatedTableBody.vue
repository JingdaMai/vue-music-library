<template>
  <tbody>
    <template v-for="(song, index) in paginatedItems">
      <tr :key="index">
        <td>{{ song.artist }}</td>
        <td>
          <slot
            name="addTitle"
            :song="song"
          />
          {{ song.title }}
        </td>
        <td>{{ song.album }}</td>
        <td>{{ song.genre }}</td>
      </tr>
    </template>
  </tbody>
</template>

<script>
export default {
  props: {
    items: {
      type: Array,
      required: true
    },
    perPage: {
      type: Number,
      required: false,
      default: 50
    }
  },
  data() {
    return {
      page: 1
    }
  },
  computed: {
    totalPages () {
      return Math.ceil(this.items.length / this.perPage);
    },
    paginatedItems () {
      return this.items.slice(0, this.page * this.perPage);
    }
  },
  watch: {
    items() {
      this.page = 1;
    }
  },
  created() {
    window.addEventListener('scroll', () => {
      if (this.page >= this.totalPages) return;
      if (window.innerHeight + window.pageYOffset >= document.body.offsetHeight - 100) {
        this.page++;
      }
    })
  }
}
</script>
