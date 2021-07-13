/* eslint-disable vue/no-use-v-if-with-v-for */
<template>
  <div role="region" aria-live="polite" aria-relevant="all">
      <div v-cloak>

        <!--PROVIDER FILTER START -->
        <div class="row tab-row">
          <a id="viewMap" v-if="listVisible" class="btn btn-primary" href="#viewMap" v-on:click="mapVisible = true, listVisible = false, navSticky = false">View by map <span class="fa fa-map" aria-hidden="true"></span></a>
          <a v-if="mapVisible" id="viewList" class="btn btn-primary" href="#viewList" v-on:click="mapVisible = false, listVisible = true, navSticky = true">View by list <span class="fa fa-list" aria-hidden="true"></span></a>
        </div>
        <div id="filters">
       
          <div id="filterRow" class="container">
            <div class="row">
              <div class="col-sm-8">

                <label for="providerSearch">Search</label>
                <input type="text" id="providerSearch" class="form-control" placeholder="Search provider names" @click="resetZoom" v-model="searchPro" />

              </div>



              <div id="viewingTotal" class="col-sm-4">
                <p class="lead" id="providerTotal" v-if="!loading && !errored">Viewing <span v-if="itemsCount == lessThan">all</span> <strong>{{itemsCount}}
                  </strong> provider<span v-if="itemsCount > 1 || itemsCount == 0">s</span></p>

              </div>
            </div>

          </div>

              <div class="col-sm-12">

                <p><a type="button" href="#filter" id="filterButton" class="lowered collapsed" data-toggle="collapse" data-target="#filter-area" aria-expanded="false" aria-controls="filter-area"  @click="resetZoom">Filters</a></p>

              </div>
              <div class="collapse mb-2" id="filter-area">
                      <section class="card card-body">
      
                      
                        <!--<button class="btn btn-xs btn-danger" v-on:click="resetForm">Clear all filters <span class="fa fa-times-circle" aria-hidden="true"></span></button>-->
                     
           
                        <div class="row">
                          <div class="col-sm-6">
                            <h4>Choose services offered</h4>
                            <div class="form-group">
                              <ul class="list-unstyled">
                                <li><label><input type="checkbox" v-model="selectedMh" name="services">
                                                                  Mental Health</label></li>
                                <li><label><input type="checkbox" v-model="selectedSud" name="services">
                                                                  Substance Use</label></li>
                                <li><label><input type="checkbox" v-model="selectedCrisis" name="services">
                                                                  Crisis services</label></li>
                                <li><label><input type="checkbox" v-model="selectedOpioid" name="services">
                                                                  Opioid</label></li>
                                <li><label><input type="checkbox" v-model="selectedDetox" name="services">
                                                                  Detox</label></li>
                                <li><label><input type="checkbox" v-model="selectedHousing" name="services">
                                                                  Housing</label></li>
                                <li><label><input type="checkbox" v-model="selectedSudres" name="services">
                                                                  Residential Substance Use treatment</label></li>
                                <li><label><input type="checkbox" v-model="selectedMhres" name="services">
                                                                  Residential Mental Health treatment</label></li>
                                <li><label><input type="checkbox" v-model="selectedInvoluntary"
                                                                      name="services"> Involuntary Committment</label></li>
                              </ul>
                            </div>
                          </div>
                          <div class="col-sm-6">
            <div class="form-group">
                            <label for="providerSelect">Select a provider by name</label>
                            <select id="providerSelect" class="form-control" v-model="selectedProvider">
                                                      <option value="">
                                                          All
                                                      </option>
                                                      <option v-for="item in info_provider" :value="item" :key="item">
                                                          {{ item }}
                                                      </option>
                                                  </select>
                            
                        </div>
                        
                            <div class="form-group">
                            <label for="locationSelect">Select by location served</label>
                            <select id="locationSelect" class="form-control" v-model="selectedLocations">
                                                      <option value="">
                                                          All
                                                      </option>
                                                      <option v-for="item in info_locations" :value="item || 'undefined'" :key="item">
                                                          {{item}}
                                                      </option>
                                                  </select>
                        </div>
                        
                        
                          <div class="form-group">
                            <label for="zipCode">Find providers by zip code</label>
                            <input type="text" id="zipCode" class="form-control" v-model="searchText" maxlength="5">
                        </div>
                 
                        <div class="form-group">
                          <h4>Choose audiences served</h4>
                          <ul class="list-unstyled">
                            <li><label><input type="checkbox" v-model="selectedAdults" name="audience">
                                                          Adults</label></li>
                            <li><label><input type="checkbox" v-model="selectedYouth" name="audience">
                                                          Youth</label></li>
                            <li><label><input type="checkbox" v-model="selectedFamilies" name="audience">
                                                          Families</label></li>
                            <li><label><input type="checkbox" v-model="selectedOlder" name="audience">
                                                          Older adults</label></li>
                          </ul>
                        </div>
                          </div>
                        </div>
                      </section>
      
                    </div>
  
          
          
          
          <div id="filterButtons" class="row">
            <div class="col-sm-12" v-if="itemsCount < lessThan" id="filterButtons">

              <ul class="list-inline m-t-sm">
                <li class="list-inline-item"><button v-if="itemsCount < lessThan" class="btn btn-xs btn-danger" v-on:click="resetForm">Clear all filters <span class="fa fa-times-circle" aria-hidden="true"></span></button></li>
                <li class="list-inline-item" v-if="selectedProvider"><button class="btn btn-xs btn-default" v-on:click="selectedProvider = ''">{{this.selectedProvider}} <span class="fa fa fa-times-circle" aria-hidden="true"></span></button></li>

                <li class="list-inline-item" v-if="selectedMh"><button class="btn btn-xs btn-default" v-on:click="selectedMh = ''">Mental Health <span class="fa fa-times-circle" aria-hidden="true"></span></button></li>
                <li class="list-inline-item"  v-if="selectedSud"><button class="btn btn-xs btn-default" v-on:click="selectedSud = ''">Substance Use <span class="fa fa fa-times-circle" aria-hidden="true"></span></button></li>
                <li class="list-inline-item"  v-if="selectedCrisis"><button class="btn btn-xs btn-default" v-on:click="selectedCrisis = ''">Crisis <span class="fa fa fa-times-circle" aria-hidden="true"></span></button></li>
                <li class="list-inline-item"  v-if="selectedOpioid"><button class="btn btn-xs btn-default" v-on:click="selectedOpioid = ''">Opioid Treatment <span class="fa fa fa-times-circle" aria-hidden="true"></span></button></li>
                <li class="list-inline-item"  v-if="selectedDetox"><button class="btn btn-xs btn-default" v-on:click="selectedDetox= ''">Detox <span class="fa fa fa-times-circle" aria-hidden="true"></span></button></li>
                <li class="list-inline-item"  v-if="selectedHousing"><button class="btn btn-xs btn-default" v-on:click="selectedHousing = ''">Housing <span class="fa fa fa-times-circle" aria-hidden="true"></span></button></li>
                <li class="list-inline-item"  v-if="selectedSudres"><button class="btn btn-xs btn-default" v-on:click="selectedSudres = ''">Residential Substance Use Treatment <span class="fa fa fa-times-circle" aria-hidden="true"></span></button></li>
                <li class="list-inline-item"  v-if="selectedMhres"><button class="btn btn-xs btn-default" v-on:click="selectedMhres= ''">Residential Mental Health Treatment <span class="fa fa fa-times-circle" aria-hidden="true"></span></button></li>
                <li class="list-inline-item"  v-if="selectedInvoluntary"><button class="btn btn-xs btn-default" v-on:click="selectedInvoluntary = ''">Involuntary Commmitment <span class="fa fa fa-times-circle" aria-hidden="true"></span></button></li>

                <li class="list-inline-item"  v-if="selectedLocations"><button class="btn btn-xs btn-default" v-on:click="selectedLocations = ''">{{this.selectedLocations}} <span class="fa fa fa-times-circle" aria-hidden="true"></span></button></li>
                <li class="list-inline-item"  v-if="searchText"><button class="btn btn-xs btn-default" v-on:click="searchText = ''">{{this.searchText}} <span class="fa fa fa-times-circle" aria-hidden="true"></span></button></li>

                <li class="list-inline-item"  v-if="selectedYouth"><button class="btn btn-xs btn-default" v-on:click="selectedYouth = ''">Youth <span class="fa fa fa-times-circle" aria-hidden="true"></span></button></li>
                <li class="list-inline-item"  v-if="selectedAdults"><button class="btn btn-xs btn-default" v-on:click="selectedAdults = ''">Adults <span class="fa fa fa-times-circle" aria-hidden="true"></span></button></li>
                <li class="list-inline-item"  v-if="selectedFamilies"><button class="btn btn-xs btn-default" v-on:click="selectedFamilies = ''">Families <span class="fa fa fa-times-circle" aria-hidden="true"></span></button></li>
                <li class="list-inline-item"  v-if="selectedOlder"><button class="btn btn-xs btn-default" v-on:click="selectedOlder = ''">Older Adults <span class="fa fa fa-times-circle" aria-hidden="true"></span></button></li>

              </ul>
            </div>
          </div>
        </div>



        <!--PROVIDER FILTER END -->
        <section id="errorMessage" v-if="errored">

          <p class="well">We're sorry, we're not able to retrieve this information at the moment, please try back later</p>

        </section>

        <!-- NO RESULTS MESSAGE START -->
        <section id="noResults" v-if="!loading && !computed_items.length && !errored">

          <div class="panel panel-danger">
            <div class="panel-heading">
              <h3 class="panel-title">Hmm... we couldn't find any results</h3>

            </div>
            <div class="panel-body">
              <p>Try refining your search or filters to see if there are any providers that match.</p>
              <p class="text-center"><button class="btn btn-xs btn-danger" v-on:click="resetForm">Clear all filters <span class="fa fa-times-circle" aria-hidden="true"></span></button></p><br>
            </div>
          </div>

        </section>
        <!-- NO RESULTS MESSAGE END -->

        <!-- CARD LIST START -->
        <section v-else>
          <!--LOADING ICON START -->
          <div class="row">
            <div class="col-sm-8">
              <div v-if="loading" class="text-center m-a-lg"><span class="fa fa-spinner fa-spin fa-3x fa-fw" aria-hidden="true"></span>
                <span class="sr-only">Loading...</span></div>
            </div>
        </div>

          <!--CARD ROW CONTAINER START -->
          <div class="row" v-for="i in Math.ceil(computed_items.length / 3)" :key="i">

            <div :key="i" v-for="(items, i) in computed_items.slice((i - 1) * 3, i * 3)" class="col-md-4 mb-4">

              <!--PROVIDER INFO CARD START -->

              <div class="shadow card h-100">
                  

                <div class="card-body">
                  <h3 class="card-title">{{ items.provider }}</h3>
                  
                  <div class="row">
                    <div class="col-sm-6">
                      <div class="contacts">
                        <p v-if="items.phone"><span class="fa fa-phone fa-color-info" aria-hidden="true"></span> <a :title="items.phone" :href="'tel:' + items.phone">{{items.phone}}</a></p>
                        <p><span class="fa fa-globe fa-color-info" aria-hidden="true"></span> <a v-if="items.website" target="_blank" :title="items.provider" :href="items.website">Website</a></p>
                        <p v-if="items.address_address"><span class="fa fa-map-marker fa-color-info" aria-hidden="true"></span> <a target="_blank" :href="'https://www.google.com/maps/dir//' + items.address_address + ', ' + items.address_city + ', ' + items.address_state + ' ' + items.address_zip">Get directions</a></p>

                      </div>
                    </div>
                    <div class="col-sm-6">

                      <div class="row">
                        <div class="col-sm-12">
                          <h4 class="m-t-0 h5" v-if="items.mental_health || items.substance_use || items.opioid || items.detox || items.crisis_services || items.involuntary || items.housing || items.sud_residential || items.mh_residential">Services</h4>
                          <ul class="list-unstyled service-list">
                            <li v-if="items.mental_health"><span class="fa fa-check-circle fa-color-success" aria-hidden="true"></span> Mental Health</li>
                            <li v-if="items.substance_use"><span class="fa fa-check-circle fa-color-success" aria-hidden="true"></span> Substance Use</li>
                            <li v-if="items.opioid"><span class="fa fa-check-circle fa-color-success" aria-hidden="true"></span> Opioid treatment</li>
                            <li v-if="items.detox"><span class="fa fa-check-circle fa-color-success" aria-hidden="true"></span> Detox</li>
                            <li v-if="items.crisis_services"><span class="fa fa-check-circle fa-color-success" aria-hidden="true"></span> Crisis Services</li>
                            <li v-if="items.involuntary"><span class="fa fa-check-circle fa-color-success" aria-hidden="true"></span> Involuntary Committment</li>
                            <li v-if="items.housing"><span class="fa fa-check-circle fa-color-success" aria-hidden="true"></span> Housing Assistance</li>
                            <li v-if="items.sud_residential"><span class="fa fa-check-circle fa-color-success" aria-hidden="true"></span> Residential substace use treatment</li>
                            <li v-if="items.mh_residential"><span class="fa fa-check-circle fa-color-success" aria-hidden="true"></span> Residential mental health treatment</li>
                          </ul>
                        </div>

                      </div>

                    </div>

                  </div>
                  <div class="row mt-3">
                    <div class="col-sm-12">
                      <p class="text-center p-t-md">{{items.address_address}}<span v-if="items.address_zip">,</span> {{items.address_city}}<span v-if="items.address_zip">,</span> {{items.address_state}} {{items.address_zip || 'Call only'}}
                      </p>
                      <p class="text-center" v-if="items.notes"><em>{{items.notes}}</em></p>
                      <hr>

                      <p v-if="items.locations"><strong>Area served:</strong> {{items.locations}}</p>

                      <ul class="list-inline">
                        <li v-if="items.adults || items.youth || items.older_adults || items.families"><strong>Groups
                                                    served:</strong></li>
                        <li v-if="items.adults">Adults</li>
                        <li v-if="items.youth">Youth</li>
                        <li v-if="items.older_adults">Older adults</li>
                        <li v-if="items.families">Families</li>
                      </ul>

                    </div>
                  </div>
                </div>
              </div>

              <!--PROVIDER CARD END -->

            </div>

          </div>
      <!-- CARD ROW CONTAINER END -->

                <!--LOADING ICON END -->
                <div v-if="mapVisible" class="row flexContainer">

                    <div class="col-md-8 col-md-push-4 p-x-0">
                      <!-- MAP START -->
                      <button type="button" id="return" @click="resetZoom()" class="resetControls btn btn-xs btn-danger">Reset zoom</button>
                      <button type="button" id="setLocation" @click="setLocation()" class="findControls btn btn-sm btn-success">Zoom to my location <span class="fa fa-crosshairs" aria-hidden="true"></span></button>
        
                      <l-map id="map" ref="map" :center="center" :bounds="bounds">
        
                        <l-tile-layer :url="url">
        
                        </l-tile-layer>
        
                        <div :key="i" v-for="(items, i) in computed_items">
        
                          <l-marker @click="selectedMapAddress = items.address_address, selectedMapCoordinates = items.address.coordinates, pinZoom()" v-if="items.address_address" :lat-lng="[items.address.coordinates[1], items.address.coordinates[0]]">
                            <l-popup>{{items.provider}}</l-popup>
                          </l-marker>
        
                        </div>
        
                      </l-map>
        
                    </div>
        
                    <div class="mapFlex col-md-4 col-md-pull-8 p-x-0">
        
                      <div class="mapLoad text-center p-a" v-if="!selectedMapAddress">
                        <h2 class="text-center">Search, filter or explore the map</h2>
                        <img class="center-block" src="https://unpkg.com/leaflet@1.2.0/dist/images/marker-icon.png">
                        <p>Click or tap a blue pin and details about that provider will appear in this space.</p>
                        <img class="center-block" style="width:25px; height:41px;" src="https://cdn.rawgit.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-red.png">
                        <p>Click or tap the "Zoom to my location" button at the bottom of the map to see providers near you. (NOTE: You'll need to enable location services on your browser or phone for this feature to work.)</p>
                        <div class="center-block text-center">
                          <button id="filterButton2" type="button" class="btn btn-success lowered" @click="resetZoom" data-toggle="modal" data-target="#myModal">Filters <span class="fa fa-sliders" aria-hidden="true"></span>
                                </button>
                          <p>Use the search bar and/or click the filters button to narrow your results.</p>
                        </div>
                      </div>
                      <div :key="i" v-for="(items, i) in computed_map" class="providerContainer flex-container">
        
                        <div v-if="selectedMapAddress" class="card h-100">
                  

                <div class="card-body h-100">
                  <h3 class="card-title">{{ items.provider }}</h3>
                  
                  <div class="row">
                    <div class="col-sm-6">
                      <div class="contacts">
                        <p v-if="items.phone"><span class="fa fa-phone fa-color-info" aria-hidden="true"></span> <a :title="items.phone" :href="'tel:' + items.phone">{{items.phone}}</a></p>
                        <p><span class="fa fa-globe fa-color-info" aria-hidden="true"></span> <a v-if="items.website" target="_blank" :title="items.provider" :href="items.website">Website</a></p>
                        <p v-if="items.address_address"><span class="fa fa-map-marker fa-color-info" aria-hidden="true"></span> <a target="_blank" :href="'https://www.google.com/maps/dir//' + items.address_address + ', ' + items.address_city + ', ' + items.address_state + ' ' + items.address_zip">Get directions</a></p>

                      </div>
                    </div>
                    <div class="col-sm-6">

                      <div class="row">
                        <div class="col-sm-12">
                          <h4 class="m-t-0 h5" v-if="items.mental_health || items.substance_use || items.opioid || items.detox || items.crisis_services || items.involuntary || items.housing || items.sud_residential || items.mh_residential">Services</h4>
                          <ul class="list-unstyled service-list">
                            <li v-if="items.mental_health"><span class="fa fa-check-circle fa-color-success" aria-hidden="true"></span> Mental Health</li>
                            <li v-if="items.substance_use"><span class="fa fa-check-circle fa-color-success" aria-hidden="true"></span> Substance Use</li>
                            <li v-if="items.opioid"><span class="fa fa-check-circle fa-color-success" aria-hidden="true"></span> Opioid treatment</li>
                            <li v-if="items.detox"><span class="fa fa-check-circle fa-color-success" aria-hidden="true"></span> Detox</li>
                            <li v-if="items.crisis_services"><span class="fa fa-check-circle fa-color-success" aria-hidden="true"></span> Crisis Services</li>
                            <li v-if="items.involuntary"><span class="fa fa-check-circle fa-color-success" aria-hidden="true"></span> Involuntary Committment</li>
                            <li v-if="items.housing"><span class="fa fa-check-circle fa-color-success" aria-hidden="true"></span> Housing Assistance</li>
                            <li v-if="items.sud_residential"><span class="fa fa-check-circle fa-color-success" aria-hidden="true"></span> Residential substace use treatment</li>
                            <li v-if="items.mh_residential"><span class="fa fa-check-circle fa-color-success" aria-hidden="true"></span> Residential mental health treatment</li>
                          </ul>
                        </div>

                      </div>

                    </div>

                  </div>
                  <div class="row mt-3">
                    <div class="col-sm-12">
                      <p class="text-center p-t-md">{{items.address_address}}<span v-if="items.address_zip">,</span> {{items.address_city}}<span v-if="items.address_zip">,</span> {{items.address_state}} {{items.address_zip || 'Call only'}}
                      </p>
                      <p class="text-center" v-if="items.notes"><em>{{items.notes}}</em></p>
                      <hr>

                      <p v-if="items.locations"><strong>Area served:</strong> {{items.locations}}</p>

                      <ul class="list-inline">
                        <li v-if="items.adults || items.youth || items.older_adults || items.families"><strong>Groups
                                                    served:</strong></li>
                        <li v-if="items.adults">Adults</li>
                        <li v-if="items.youth">Youth</li>
                        <li v-if="items.older_adults">Older adults</li>
                        <li v-if="items.families">Families</li>
                      </ul>

                    </div>
                  </div>
                </div>
              </div>
                      </div>
                    </div>
                  </div>
      </section>

</div>
</div>
</template>

<script>
import { LMap, LTileLayer, LMarker, LPopup} from 'vue2-leaflet';
import axios from 'axios';
export default {
  name: 'HelloWorld',
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPopup
  },
  data() {
    return {
      navSticky: true,
      mapVisible: false,
      listVisible: true,
      findLocation: false,
      id: "map",
      center: [0, 0],
      bounds: [
        [49.004521, -117.037357],
        [45.645316, -124.588534]
      ],
      url:
        "https://server.arcgisonline.com/ArcGIS/rest/services/World_Topo_Map/MapServer/tile/{z}/{y}/{x}",
      searchPro: "",
      selectedZip: "",
      selectedProvider: "",
      selectedMh: "",
      selectedSud: "",
      selectedCrisis: "",
      selectedOpioid: "",
      selectedDetox: "",
      selectedHousing: "",
      selectedSudres: "",
      selectedMhres: "",
      selectedInvoluntary: "",
      selectedServices: "",
      selectedLocations: "",
      selectedCoordinates: "",
      selectedAddress: "",
      selectedYouth: "",
      selectedAdults: "",
      selectedFamilies: "",
      selectedOlder: "",
      searchText: "",
      selectedMapProvider: "",
      selectedMapAddress: "",
      selectedMapCoordinates: "",

      info: [],
      loading: true,
      errored: false
    };
  },

  mounted() {
    axios
      .get(
        "//data.kingcounty.gov/resource/f5x7-bnzp.json?$order=provider%20ASC"
      )
      .then(response => {
        this.info = response.data;
      })
      .catch(error => {
        console.log(error);
        this.errored = true;
      })
      .finally(() => (this.loading = false));

    window.addEventListener("keyup", event => {
      if (event.keyCode === 13) {
        this.pinZoom();
      }
    });
  },

  computed: {
    itemsCount() {
      // computed_items is computed
      return this.computed_items.length;
    },
    lessThan() {
      return this.info.length;
    },

    computed_items: function() {
      let filterZip = this.selectedZip,
        filterProvider = this.selectedProvider,
        filterMh = this.selectedMh,
        filterSud = this.selectedSud,
        filterCrisis = this.selectedCrisis,
        filterOpioid = this.selectedOpioid,
        filterDetox = this.selectedDetox,
        filterHousing = this.selectedHousing,
        filterSudres = this.selectedSudres,
        filterMhres = this.selectedMhres,
        filterInvoluntary = this.selectedInvoluntary,
        filterLocations = this.selectedLocations,
        filterAddress = this.selectedAddress,
        filterCoordinates = this.selectedCoordinates,
        filterYouth = this.selectedYouth,
        filterAdults = this.selectedAdults,
        filterFamilies = this.selectedFamilies,
        filterOlder = this.selectedOlder,
        filterSearch = this.searchText,
        totalSearch = this.searchPro;

      return this.info.filter(function(item) {
        //SEARCH FUNCTION

        let filtered = true;

        if (filterZip && filterZip.length > 0) {
          if (item.address_zip != undefined) {
            filtered = item.address_zip == filterZip;
          }
          if (item.address_zip === undefined) {
            return false;
          }
          filtered = item.address_zip == filterZip;
        }

        if (filtered) {
          if (filterProvider && filterProvider.length > 0) {
            filtered = item.provider == filterProvider;
          }
        }
        if (filtered) {
          if (filterAddress && filterAddress.length > 0) {
            filtered = item.address_address == filterAddress;
          }
        }
        if (filtered) {
          if (filterLocations && filterLocations.length > 0) {
            if (item.locations != undefined) {
              filtered = item.locations == filterLocations;
            }
            if (item.locations === undefined) {
              return false;
            }
            filtered = item.locations == filterLocations;
          }
        }
        if (filtered) {
          if (filterCoordinates && filterCoordinates.length > 0) {
            if (item.address.coordinates != undefined) {
              filtered = item.address.coordinates == filterCoordinates;
            }
            if (item.address.coordinates === undefined) {
              return false;
            }
            filtered = item.address.coordinates == filterCoordinates;
          }
        }
        if (filtered) {
          if (filterMh && filterMh === true) {
            filtered = item.mental_health == filterMh;
          }
        }
        if (filtered) {
          if (filterSud && filterSud === true) {
            filtered = item.substance_use == filterSud;
          }
        }
        if (filtered) {
          if (filterCrisis && filterCrisis === true) {
            filtered =
              item.crisis_services ||
              (item.locations === "undefined") == filterCrisis;
          }
        }
        if (filtered) {
          if (filterOpioid && filterOpioid === true) {
            filtered = item.opioid == filterOpioid;
          }
        }
        if (filtered) {
          if (filterDetox && filterDetox === true) {
            filtered = item.detox == filterDetox;
          }
        }
        if (filtered) {
          if (filterHousing && filterHousing === true) {
            filtered = item.housing == filterHousing;
          }
        }
        if (filtered) {
          if (filterSudres && filterSudres === true) {
            filtered = item.sud_residential == filterSudres;
          }
        }
        if (filtered) {
          if (filterMhres && filterMhres === true) {
            filtered = item.mh_residential == filterMhres;
          }
        }
        if (filtered) {
          if (filterInvoluntary && filterInvoluntary === true) {
            filtered = item.involuntary == filterInvoluntary;
          }
        }
        if (filtered) {
          if (filterYouth && filterYouth === true) {
            filtered = item.youth == filterYouth;
          }
        }
        if (filtered) {
          if (filterAdults && filterAdults === true) {
            filtered = item.adults == filterAdults;
          }
        }
        if (filtered) {
          if (filterFamilies && filterFamilies === true) {
            filtered = item.families == filterFamilies;
          }
        }
        if (filtered) {
          if (filterOlder && filterOlder === true) {
            filtered = item.older_adults == filterOlder;
          }
        }
        if (filtered) {
          if (filterSearch && filterSearch.length > 1) {
            if (item.address_zip != undefined) {
              filtered = item.address_zip.includes(filterSearch);
            }
            if (item.address_zip === undefined) {
              return false;
            }
          }
        }
        if (filtered) {
          if (totalSearch.length > 0) {
            if (item.provider != undefined) {
              filtered = item.provider
                .toLowerCase()
                .includes(totalSearch.toLowerCase());
            }
            if (item.provider === undefined) {
              return false;
            }
          }
        }
        return filtered;
      });
    },
    computed_map: function() {
      let mapProvider = this.selectedMapProvider,
        mapAddress = this.selectedMapAddress,
        mapCoordinates = this.selectedMapCoordinates;

      return this.info.filter(function(item) {
        //SEARCH FUNCTION

        let filteredMap = true;

        if (filteredMap) {
          if (mapProvider && mapProvider.length > 0) {
            filteredMap = item.provider == mapProvider;
          }
        }
        if (filteredMap) {
          if (mapAddress && mapAddress.length > 0) {
            filteredMap = item.address_address == mapAddress;
          }
        }
        if (filteredMap) {
          if (mapCoordinates && mapCoordinates.length > 0) {
            if (item.address.coordinates != undefined) {
              filteredMap = item.address.coordinates == mapCoordinates;
            }
            if (item.address.coordinates === undefined) {
              return false;
            }
            filteredMap = item.address.coordinates == mapCoordinates;
          }
        }
        return filteredMap;
      });
    },
    info_provider: function() {
      return [...new Set(this.computed_items.map(i => i.provider))];
    },

    info_zip: function() {
      return [
        ...new Set(
          this.computed_items
            .map(i => i.address_zip)
            .slice()
            .sort()
        )
      ];
    },
    info_locations: function() {
      return [
        ...new Set(
          this.computed_items
            .map(i => i.locations)
            .slice()
            .sort()
        )
      ];
    }
  },
  methods: {
    resetForm: function() {
      this.selectedMh = "";
      this.selectedSud = "";
      this.selectedCrisis = "";
      this.selectedOpioid = "";
      this.selectedDetox = "";
      this.selectedHousing = "";
      this.selectedSudres = "";
      this.selectedMhres = "";
      this.selectedInvoluntary = "";
      this.selectedAdults = "";
      this.selectedYouth = "";
      this.selectedOlder = "";
      this.selectedFamilies = "";
      this.selectedProvider = "";
      this.selectedLocations = "";
      this.selectedCoordinates = "";
      this.selectedAddress = "";
      this.searchText = "";
      this.searchPro = "";
      this.selectedMapProvider = "";
      this.selectedMapAddress = "";
      this.selectedMapCoordinates = "";
      if (this.mapVisible == true) {
        this.resetMap();
      }
    },
    resetMap() {
      const mapComponent = this.$refs.map.mapObject;
      mapComponent.closePopup();
      this.bounds = LMap.latLngBounds([
        [49.004521, -117.037357],
        [45.645316, -124.588534]
      ]);
    },
    resetZoom() {
      if (this.mapVisible) {
        this.bounds = LMap.latLngBounds([
          [49.004521, -117.037357],
          [45.645316, -124.588534]
        ]);
      }
    },
    pinZoom: function() {
      const mapComponent = this.$refs.map.mapObject;
      var latLngs = this.selectedMapCoordinates;
      var lats = latLngs[1];
      var longs = latLngs[0];
      var markerBounds = LMap.latLng([lats, longs]);
      var zoomLev = mapComponent.getZoom();
      if (zoomLev < 12) {
        mapComponent.setView(markerBounds, 12);
      }
    },

    setLocation: function() {
      const mapComponent = this.$refs.map.mapObject;
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          function(position) {
            let latlng = new LMap.LatLng(
              position.coords.latitude,
              position.coords.longitude
            );
            document.addEventListener(
              "click",
              function(event) {
                var imageSrc = event.target.src;
                
                if (
                  imageSrc ==
                  "https://cdn.rawgit.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-red.png"
                )
                  mapComponent.setView(latlng, 12);
              },
              false
            );

            mapComponent.setView(latlng, 12);
            console.log(latlng);

            var redIcon = new LMap.Icon({
              iconUrl:
                "https://cdn.rawgit.com/pointhi/leaflet-color-markers/master/img/marker-icon-2x-red.png",
              shadowUrl:
                "https://cdnjs.cloudflare.com/ajax/libs/leaflet/0.7.7/images/marker-shadow.png",
              iconSize: [25, 41],
              iconAnchor: [12, 41],
              popupAnchor: [1, -34],
              shadowSize: [41, 41]
            });
            // eslint-disable-next-line no-unused-vars
            var redPin = new LMap.marker(latlng, { icon: redIcon })
              .setZIndexOffset(5000)
              .addTo(mapComponent)
              .bindPopup("Your location");
          },
          function(error) {
            console.log(error.message);
            alert(
              "You'll need to enable location sharing in your browser or on your device for this service to work."
            );
          }
        );
      } else {
        alert("Geolocation is not supported by this browser.");
      }
    }
  }

}
</script>
