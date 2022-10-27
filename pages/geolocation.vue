<template>
    <div id="map"></div>
</template>

<script>
import * as L from 'leaflet';
import 'leaflet/dist/leaflet.css';
import 'leaflet/dist/images/marker-shadow.png'
import 'leaflet/dist/images/marker-icon-2x.png'

export default {
    mounted(){
        if(process.client){
            navigator.geolocation.watchPosition(position => {
                if(!this.marker){
                    this.marker = L.marker([position.coords.latitude, position.coords.longitude]);
                    this.marker.addTo(this.map);
                } else {
                    this.marker.setLatLng([position.coords.latitude, position.coords.longitude]);
                }
                this.map.flyTo([position.coords.latitude, position.coords.longitude], 18);
            });
            this.map = L.map('map').setView([59.505, 24.09], 18);
            L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png', {
                maxZoom: 19,
                attribution: '&copy; <a href="http://www.openstreetmap.org/copyright">OpenStreetMap</a>'
            }).addTo(this.map);
        }
    },
    data(){
        return {
            map: null,
            marker: null
        }
    }
}
</script>

<style>
#map { height: 80vh; }
</style>