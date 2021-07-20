<template>
  <div id="app">
    <ProvidersLookup/>
  </div>
</template>

<script>
import ProvidersLookup from './components/ProvidersLookup.vue'

export default {
  name: 'App',
  components: {
    ProvidersLookup
  }
}
</script>

<style>
body {
  overflow-x: visible !important;
}
[v-cloak] {
  display: none;
}

#errorMessage,
#noResults {
  margin-top: 2rem;
}
.flex-container {
  display: flex;
  flex-direction: column;
  flex-basis: auto;
  margin-bottom: 2rem;
}
#search-icon {
  vertical-align: middle;
}
.card {
  background-color: #ffffff;
}
.card-body {
  padding: 1.5rem;
}
.accordion .card .card-header button span, .accordion .card:not(:last-of-type) .card-header button span {
    right: unset;
    top: 10px;
}
#filterButtons li button {
    font-size: 14px;
}
#filterButtons .material-icons {
    font-size: 14px;
}
.mapFlex {
  width: 100%;
  overflow-y: hidden;
  overflow-x: hidden;
  height: 500px;
  margin-left: 0;
  margin-right: 0;
  flex-direction: column;
}
#viewingTotal {
  align-items: flex-end;
display: flex;
flex-direction: row;
}
.vue2leaflet-map {
  width: 100%;
  min-height: 500px !important;
  z-index: 10;
}
.findControls {
  z-index: 1000;
  position: absolute;
  bottom: -45px;
  left: 50%;
  -webkit-transform: translateX(-50%);
  transform: translateX(-50%);
}
.leaflet-marker-icon:focus {
  border-bottom: 3px solid blue;
  z-index: 10000 !important;
}

@media only screen and (min-width: 992px) {
  .flex-container {
    flex-direction: row;
  }
}

@media only screen and (max-width: 768px) {
  .vue2leaflet-map {
    min-height: 300px !important;
  }


  #filterButtons {
    display: none;
  }
  #viewingTotal {
    display: none;
  }



}

</style>
