<template>
  <div>
    <h3>{{ lat }}, {{ long }}</h3>
    <h3>computed: {{ myCoordinates() }}</h3>
    <!-- 
      @keyup.enter="submit"
      v-on:keyup.enter="submit"
      @click="counter += 1"
      v-on:click="counter += 1"
    -->
    <input type="number" v-model="lat" @keyup.enter="addMarker">
    <input v-model="long" @keyup.enter="addMarker">
    <button @click="addMarker">Add marker</button>
    <br/>
    <br/>
    <div id="map">    
    </div>
    <ul id="table">
      <li v-for="point in geoPoints" :key="point">
        Lat: {{ point.latLng[0] }}, Long: {{ point.latLng[1] }}
      </li>
    </ul>
  </div>
</template>

<script>
var myMap;

export default { 
  data: function() {
    return {
      map: null,
      lat: 0,
      long: 0,
      geocoordinates: [
        [-1, 33],
        [0, 20],
      ],
      geoPoints:[
        {
          latLng: [10,10],
          marker: null
        }
      ],
      currentMarker: null
    }
  },
  mounted() {
    /*
      let script = document.createElement('script')
      script.setAttribute('src', 'https://www.somecdn.com/foo/bar.js')
      document.head.appendChild(script)
    */

    myMap = L.map('map').setView([0.001, -0.001], 13);
    myMap.setZoom(3);

    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
    attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
    }).addTo(myMap);

    L.marker([this.lat, this.long]).addTo(myMap).bindPopup('Some location');
  },
  methods:{
    myCoordinates: function(){
      var tempLat = parseFloat(this.lat, 10) || 0;
      var tempLong = parseFloat(this.long, 10) || 0;
      if(myMap){
        this.currentMarker = L.marker([tempLat, tempLong]); 
      }
      return 'Lat: ' + tempLat + ', ' + tempLong;
    },
    addMarker: function(){

      if(!this.currentMarker){
        this.myCoordinates();
      }
      
      var geoPoint = {
        latLng: [this.lat, this.long],
        marker: this.currentMarker
      }
      this.geoPoints.push(geoPoint);
      this.currentMarker.on("popupopen", onPopupOpen);
      this.currentMarker.addTo(myMap).bindPopup('Changed!');
    }
  }
}

function onPopupOpen(){
  var tempMarker = this;
  console.log(tempMarker, tempMarker.getLatLng());
}
</script>

<style>
/*@import 'https://unpkg.com/leaflet@1.3.1/dist/leaflet.css';*/

#map{
  position: relative;
  float: left;
  background-color: #cecece;
  width: 70vw;
  height: 80vh;
}

#table{
  position: relative;
  float: left;
}

</style>
