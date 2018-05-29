<template>
    <div>
        <h1>this is google map</h1>
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

    </div>
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


    export default{
        data(){
            return {
                center: {lat: 23.8103, lng: 90.4125},
                markers: [
                    {position: {lat: 10.0, lng: 10.0}}
                ],
                getMap: this.$root.mapping,
                description: '',
                latLng: {},
                place: null,
                currentPosition: {
                    lat:0,
                    lng:0
                }
            }
        },

        mounted(){
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(function (position) {
                    this.currentPosition.lat = position.coords.latitude
                    this.currentPosition.lng = position.coords.longitude
                    let pos = {
                        lat: position.coords.latitude,
                        lng: position.coords.longitude
                    };
                    this.center.lat = pos.lat;
                    this.center.lng = pos.lng;
                    this.markers[0].position.lat = pos.lat;
                    this.markers[0].position.lng = pos.lng;

                    this.geocodeLatLng(new google.maps.Geocoder, pos, google.maps.InfoWindow);

                }.bind(this));
            }
        },
        methods: {
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
        },
    }
</script>