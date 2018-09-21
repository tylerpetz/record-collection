<template>
  <div>
    <div v-for="(lp, l) in lps" :key="l">
      {{ lp }}
    </div>
  </div>
</template>

<script>
export default {
  name: "RecordList",
  props: {
    lps: Array
  },
  data() {
    return {
      albums: []
    };
  },
  watch: {
    lps: function () {
      this.filterAlbums();
    }
  },
  methods: {
    filterAlbums: function () {
      let fullAlbums = this.lps
        .map((album) => {
          return {
            artist: album.artist,
            title: album.name,
            url: album.url,
            image: album.image.pop()
          };
        });

      this.albums = fullAlbums;
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
