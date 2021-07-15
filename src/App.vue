<template>
  <div id="app">
    <HelloWorld/>
  </div>
</template>

<script>
import HelloWorld from './components/ProvidersLookup.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
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
  bottom: -35px;
  left: 50%;
  -webkit-transform: translateX(-50%);
  transform: translateX(-50%);
}
.leaflet-marker-icon:focus {
  border-bottom: 3px solid blue;
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
  .providerHeader {
    height: 60px;
  }
  .providerHeader h3 {
    color: white;
    font-size: 16px;
  }
  .leaflet-popup h3 {
    font-size: 14px;
    max-width: 75%;
  }
}

</style>
