<template>
  <div id="app">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

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
/*
#filterRow {
  padding-bottom: 1rem;
}

#filterButton,
#providerTotal {
  margin-top: 25px;
}
.sticky {
  background: #193440;
  padding: 2rem;
  position: -webkit-sticky;
  position: sticky;
  top: 0;
  z-index: 110;
  box-shadow: 0px 9px 6px -6px rgba(119, 119, 119, 0.4392156862745098);
}

ul.service-list li {
  padding-bottom: 1rem;
}
.providerCard {
  margin-top: 2rem;
  border: 0.3px #6f6f6c26 solid;
  padding: 0 2rem 2rem 2rem;
  box-shadow: 0px 8px 20px 0px rgba(119, 119, 119, 0.4392156862745098);
  height: 100%;
}
.providerCardMap {
  padding: 0 2rem 2rem 2rem;
  box-shadow: none;
  height: 500px;
  width: 100%;
}
.providerHeader {
  margin: 0 -2rem 2rem -2rem;
  padding: 0 5px 0 5px;
  background: #4183a0;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  height: 80px;
}
.providerHeader h3 {
  color: white;
  font-size: 20px;
}
.providerContent {
  display: inherit;
  justify-content: space-evenly;
}

#providerSearch {
  display: inline-block;
}
#providerSearch::-ms-clear {
  display: none;
}
*/
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
.mapFlex {
  width: 100%;
  overflow-y: hidden;
  overflow-x: hidden;
  height: 500px;
  margin-left: 0;
  margin-right: 0;
  flex-direction: column;
}

.vue2leaflet-map {
  width: 100%;
  min-height: 500px !important;
  z-index: 10;
}
.findControls {
  z-index: 1000;
  position: absolute;
  bottom: 1px;
  left: 50%;
  -webkit-transform: translateX(-50%);
  transform: translateX(-50%);
}
.resetControls {
  z-index: 1000;
  position: absolute;
  top: 0;
  right: 0;
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
  .mobile-filters {
    display: block;
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
