<script>
import mapboxgl from 'mapbox-gl'; 
import Navbar from "./components/Navbar.vue"
import markers from "./assets/four.json"

export default {
  components: {Navbar},
  data() {
    return {
      map: {},
      markers: [],

      markerTitle: '',
      markerDescription: '',
      markerPrice: '',
      markerImage: '',
    }
  },
  methods: {
    showInfoOnOffcanvas(index) {
      this.markerTitle = markers.features[index].properties.title;
      this.markerDescription = markers.features[index].properties.description;
      this.markerPrice = markers.features[index].properties.price;
      this.markerImage = markers.features[index].properties.image;
    },
    addMakersToMap(markeres) {

      for (let i=0; i<markeres.length; i++) {
        this.markers.push(
          new mapboxgl.Marker({color: markeres[i].properties.color})
          .setLngLat([markeres[i].geometry.coordinates[0], markeres[i].geometry.coordinates[1]])
          .setPopup(new mapboxgl.Popup().on('open', () => {document.getElementById(`button${i}`).addEventListener('click', () => {this.showInfoOnOffcanvas(i)} )}).setHTML(`<div class=""> <h5 class="">${markeres[i].properties.title}</h5> <h6>${markeres[i].properties.price} $</h6> <button id="button${i}" class="btn btn-primary btn-sm" data-bs-toggle="offcanvas" href="#offcanvas" role="button" aria-controls="offcanvas">See details</button> </div>`))
          .addTo(this.map)
        )
      }
    },
    applyFilter(data) {
      // console.log('filtering  on app');
      let newmarkers = [];
      for (let i=0; i<this.markers.length; i++) {
        this.markers[i].remove();
      }; 
      if (data.color == '' && data.priceRange == "") {
        for (let j=0; j<markers.features.length; j++) {
          newmarkers.push(markers.features[j])
        };
      }  else if (data.color == '') {
       for (let j=0; j<markers.features.length; j++) {
          if (markers.features[j].properties.price >= data.priceRange.from && markers.features[j].properties.price <= data.priceRange.to) {
            newmarkers.push(markers.features[j])
          }
        };
      } else if (data.priceRange == "") {
        // color filter
        for (let j=0; j<markers.features.length; j++) {
          if (markers.features[j].properties.color == data.color) {
            newmarkers.push(markers.features[j])
          }
        };
      } else if (data.color != '' || data.priceRange != "") {
        for (let j=0; j<markers.features.length; j++) {
          if (
            markers.features[j].properties.color == data.color && 
            markers.features[j].properties.price >= data.priceRange.from && 
            markers.features[j].properties.price <= data.priceRange.to
          ) {
            newmarkers.push(markers.features[j])
          }
        };
      }

      this.addMakersToMap(newmarkers)
      
    },
    adjustMapHeight() {
      var screenHeight = document.documentElement.clientHeight,
        navbarHeightpx = getComputedStyle(document.getElementById('navbarSection')).height,
        navbarHeight = parseInt(navbarHeightpx.split('px')[0]),
        mapHeight = screenHeight - navbarHeight;
      document.getElementById('map').style.height = `${mapHeight}px`;
    },
  },
  async mounted() {

    const response = await fetch("/four.json");
    const markers = await response.json();


    this.adjustMapHeight();
    
    mapboxgl.accessToken = 'MAPBOX-TOKEN';
    this.map = new mapboxgl.Map({
      container: 'map', // container ID
      style: 'mapbox://styles/mapbox/streets-v12', // style URL
      // style: 'mapbox://styles/mapbox/navigation-night-v1', // style URL
      center: [-77.04, 38.907], // starting position [lng, lat]
      zoom: 12, // starting zoom
      // attributionControl: false,
    });
    this.map.addControl(new mapboxgl.NavigationControl(), 'bottom-right');
    this.map.addControl(new mapboxgl.ScaleControl(), 'bottom-left');

    let newmarkers = [];
    for (let j=0; j<markers.features.length; j++) {
      newmarkers.push(markers.features[j])
    };
    this.addMakersToMap(newmarkers)

  }

}

</script>

<template>


<!-- NAVBAR SECTION -->
<Navbar v-on:apply-filter="applyFilter($event)" :title="markerTitle" :description="markerDescription" :price="markerPrice" :image="markerImage"/>



<!-- MAP SECTION  -->
<div id="map" style="width: 100%">
  <nav id="menu" class="position-absolute top-0 end-0 ">
    <button type="button" class="btn btn-info m-2 border border-dark" data-bs-toggle="modal" data-bs-target="#filterModal"> Filter</button>
  </nav>
</div>
  


</template>

<style >
 #menu {
  z-index: 1;
 }
    
</style>
