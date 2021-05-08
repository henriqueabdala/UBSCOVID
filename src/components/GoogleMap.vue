<template>
  <div>
    <GmapMap :center="center" :zoom="12" id="map">
      <GmapMarker
        :key="index"
        v-for="(m, index) in markers"
        :position="m.position"
        :clickable="true"
        :draggable="true"
        @click="center = m.position"
      />
    </GmapMap>
  </div>
</template>

<script>
export default {
  name: "GoogleMap",
  data() {
    return {
      center: { lat: 45.508, lng: -73.587 },
      currentPlace: null,
      markers: [
        {
          position: {
            lat: 48.16091,
            lng: 16.38333,
          },
        },
        {
          position: {
            lat: 48.17427,
            lng: 16.32962,
          },
        },
      ],
      places: [],
    };
  },
  mounted() {
    this.geolocate();
    this.getUBS();
  },
  methods: {
    geolocate() {
      navigator.geolocation.getCurrentPosition((position) => {
        this.center = {
          lat: position.coords.latitude,
          lng: position.coords.longitude,
        };
      });
    },
    getUBS() {
      const myHeaders = new Headers();
      const requestOptions = {
        method: "GET",
        headers: myHeaders,
        redirect: "follow",
      };
      myHeaders.append("Content-Type", "application/json");
      myHeaders.append(
        "Authorization",
        "Bearer 4473dca6b489cc15b867f952c792f5c0"
      );
      fetch(
        "https://gatewayapi.prodam.sp.gov.br:443/saude/ubs/v1.0/",
        requestOptions
      )
        .then((response) => response.json())
        .then((result) => {
          console.log(this.makers);
          result.map((r) => {
            this.markers.push({
              position: {
                lat: r.geoLocalizacaoUSB.latitude,
                lng: r.geoLocalizacaoUSB.longitude,
              },
            });
          });
        })
        .catch((error) => console.log("error", error));
    },
  },
};
</script>
<style lang="sass">
#map
  width: 100%
  height: 100vh
</style>