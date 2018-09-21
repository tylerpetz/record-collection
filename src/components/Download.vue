<template>
  <span>Download Stuff</span>
</template>

<script>
import axios from "axios";
import RecordList from "@/components/RecordList.vue";

export default {
  name: "RecordCollection",
  props: {
    releases: Array
  },
  data() {
    return {
      lps: [],
      promises: []
    };
  },
  watch: {
    releases: function() {
      this.populateReleases();
    }
  },
  beforeCreate() {
    axios
      .get("https://api.discogs.com/users/hatfone/collection/folders/0/releases?page=1&per_page=200")
      .then(response => {
        console.log(response);
        this.releases = response.data.releases;
      })
      .catch(error => {
        console.log(error);
        this.error = true;
      })
      .finally(() => {
        this.loading = false;
      });
  },
  methods: {
    populateReleases: function() {
      let albums = this.releases
        .map((release) => {
          return {
            formats: release.basic_information.formats[0].descriptions,
            id: release.id,
            artist: release.basic_information.artists[0].name,
            title: release.basic_information.title
          };
        })
        .filter(release => release.formats.includes("LP"));

      let albumUrls = albums
        .map(album => `http://ws.audioscrobbler.com/2.0/?method=album.getinfo&api_key=${process.env.VUE_APP_LAST_FM_KEY}&artist=${album.artist}&album=${album.title}&format=json`);

      albumUrls.forEach(url => {
        this.promises.push(axios.get(url));
      });

      axios.all(this.promises).then(results => {
        results.forEach(res => {
          this.lps.push(res.data.album);
        })
      });
    },
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
