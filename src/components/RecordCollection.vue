<template>
  <div>
    Sup
  </div>
</template>

<script>
export default {
  name: "RecordCollection",
  props: {
    releases: Array
  },
  data() {
    return {
      albums: [],
    }
  },
  methods: {
    parseAlbums: function() {
      console.log(this.releases);
      //this.releases.map(release => console.log(release));
    }
  },
  created() {
    this.parseAlbums();
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
