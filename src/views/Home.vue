<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <RecordCollection :releases="releases" />
  </div>
</template>

<script>
import RecordCollection from "@/components/RecordCollection.vue";
import sampleData from "@/assets/sample.json";

export default {
  name: "home",
  data() {
    return {
      releases: sampleData
    };
  },
  components: {
    RecordCollection
  }
};
</script>
