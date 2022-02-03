<template>
 <v-text-field
        placeholder="Search for a city"
        v-model="query"
        @keypress="fetchCity"
      ></v-text-field>
</template>

<script>
export default {
data() {
    return {
      center: [24.81947057114111, 54.660758972167976],
      accessToken:
        "your_access_token",
      url_base: "https://api.mapbox.com/geocoding/v5/mapbox.places",
      query: "",
      city: {},
      lat: "",
      lon: "",
    };
  },
  methods: {
    fetchCity(e) {
      if (e.key == "Enter") {
        fetch(
          `${this.url_base}/${this.query}.json?access_token=${this.accessToken}`
        )
          .then((res) => {
            return res.json();
          })
          .then(this.setResults);
      }
    },
    setResults(results) {
      this.city = results;
      if (this.city.features != "undefined") {
        this.setCenter(
          `${this.city.features[0].center[0]}`,
          `${this.city.features[0].center[1]}`
        );
      }
      console.log("the value of center:", this.center);
    },
    setCenter(lat, lon) {
      this.lat = lat;
      this.lon = lon;
      this.center = [lat, lon];
      this.mapM.flyTo(new L.LatLng(lon, lat), 10);
    },
  },
};
</script>
