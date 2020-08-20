<template>
    <h1>Container that fetch and distribute data to the graphs</h1>
</template>

<script>
import axios from 'axios';

export default {
  name: 'GraphContainer',
  props: {
  },
  created() {
    // fetch the data when the view is created and the data is
    // already being observed
    this.fetchData();
  },
  methods: {
    fetchData() {
      this.error = this.post = null;
      this.loading = true;
      console.log("tst");
      axios
        .get(
          "https://data.cityofchicago.org/resource/fg6s-gzvg.json?from_station_id=418"
        )
        .then(response => {
            console.log(response.data)
        })
    }
  }
}
</script>
<style scoped>
</style>