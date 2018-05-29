<template>
  <q-page class="flex flex-center">
    <GmapMap
  :center="center"
  :zoom="13"
  map-type-id="terrain"
  style="width: 100vh; height: 90vh">
   <GmapMarker
    icon='http://maps.google.com/mapfiles/ms/icons/green-dot.png'
    :key="index"
    v-for="(m, index) in markers"
    :position="m.geometry.location"
    :clickable="true"
    @click="center=m.geometry.location"
  />
</GmapMap>
  </q-page>
</template>

<style>
</style>

<script>
import Vue from 'vue'
import axios from 'axios'
const VueGoogleMaps = require('vue2-google-maps')

Vue.use(VueGoogleMaps, {
  load: {
    key: 'AIzaSyD0jJtSC0W5bI1HP8Y0Lgh19cMSApxZoVQ'
  }
})



export default {
  name: 'PageIndex',
   data() { 
          return {
          center: {lat: 23.8103, lng: 90.4125},
          markers: [],
          getMap: this.$root.mapping,
          description: '',
          latLng: {},
          place: null
        }
      },
      mounted(){
        let self = this;
        if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(function (position) {
                    let pos = {
                        lat: position.coords.latitude,
                        lng: position.coords.longitude
                    };
                    self.center.lat = pos.lat;
                    self.center.lng = pos.lng;
                    self.markers[0].position.lat = pos.lat;
                    self.markers[0].position.lng = pos.lng;
                    self.markers.position.push({"lat": pos.lat, "lng":pos.lng })
                    self.geocodeLatLng(new google.maps.Geocoder, pos, google.maps.InfoWindow);
                  
                }.bind(this))
            }

        this.getPlaces();
      },
      methods:{
        getPlaces() {
          axios.get('https://api.myjson.com/bins/17j1ta').then(res => {
            console.log(res.data.results)
            this.markers = res.data.results
          })
          .catch(error => {

          });
        },
        setDescription(description){
                this.description = description
            },
            setPlace(place){
                this.latLng = {
                    lat: place.geometry.location.lat(),
                    lng: place.geometry.location.lng(),
                }
            },
            geocodeLatLng(geocoder, map, infowindow){
                geocoder.geocode({'location':this.center}, function(results, status){
                    console.log(results, status); 
                });
            }
      }
}
</script>
