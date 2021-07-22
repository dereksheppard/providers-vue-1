/* eslint-disable vue/no-use-v-if-with-v-for */
<template>
  <div role="region" aria-live="polite" aria-relevant="all" class="container">
      <div v-cloak>

        <!--PROVIDER FILTER START -->
        <div class="row">
          <div class="col">
          <a id="viewMap" v-if="listVisible" class="btn btn-link" href="#viewMap" v-on:click="mapVisible = true, listVisible = false" aria-label="View map">View by map <span class="fa fa-map" aria-hidden="true"></span></a>
          <a v-if="mapVisible" id="viewList" class="btn btn-link" href="#viewList" v-on:click="mapVisible = false, listVisible = true" aria-label="View list">View by list <span class="fa fa-list" aria-hidden="true"></span></a>
        </div>
        </div>




        <div id="filters">
          <div id="filterRow">
            <div class="row">
              <div class="col-sm-8">
                <span class="material-icons" id="search-icon">search</span>  
                <label for="providerSearch">Search</label>
                <input type="text" id="providerSearch" class="form-control" placeholder="Search provider names" v-model="searchPro" />

              </div>



              <div id="viewingTotal" class="col-sm-4">
                <p class="align-bottom" id="providerTotal" v-if="!loading && !errored">Viewing <span v-if="itemsCount == lessThan">all</span> <strong>{{itemsCount}}
                  </strong> provider<span v-if="itemsCount > 1 || itemsCount == 0">s</span></p>

              </div>
              <div class="col">
                <div class="accordion" id="filterAccordion">
                <div class="card">
              <div class="card-header" id="filterHeader">
                <h5 class="mb-0"><button class="btn btn-link collapsed" data-toggle="collapse" data-target="#dataTargetFilters" aria-expanded="true" aria-controls="dataTargetFilters">Filters<span tabindex="0" class="material-icons scroll-down-arrow">expand_more</span></button></h5>
                </div>
                <div id="dataTargetFilters" class="collapse" aria-labelledby="filterHeader" data-parent="#filterAccordion">
                  <div class="card-body">
                         <div class="row">
                          <div class="col-sm-6">
                            <label for="service-list">Choose services offered</label>
                            <div class="form-group">
                              <ul class="list-unstyled" id="service-list">
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
                          <label for="audience-list">Choose audiences served</label>
                          <ul class="list-unstyled" id="audience-list">
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

                  </div>
                  </div>
                  </div>
              </div>
              </div>
            </div>

          </div>

          
          
          
          <div id="filterButtons" class="row">
            <div class="col-sm-12" v-if="itemsCount < lessThan">

              <ul class="list-inline m-t-sm">
                <li class="list-inline-item"><button v-if="itemsCount < lessThan" class="btn btn-link" v-on:click="resetForm" aria-label="Clear all filters">Clear all filters <span class="material-icons">
cancel
</span></button></li>
                <li class="list-inline-item" v-if="selectedProvider"><button class="btn btn-link" v-on:click="selectedProvider = ''" aria-label="Clear this filter">{{this.selectedProvider}} <span class="material-icons">
cancel
</span></button></li>
                <li class="list-inline-item" v-if="selectedMh"><button class="btn btn-link" v-on:click="selectedMh = ''" aria-label="Clear this filter">Mental Health <span class="material-icons">
cancel
</span></button></li>
                <li class="list-inline-item"  v-if="selectedSud"><button class="btn btn-link" v-on:click="selectedSud = ''" aria-label="Clear this filter">Substance Use <span class="material-icons">
cancel
</span></button></li>
                <li class="list-inline-item"  v-if="selectedCrisis"><button class="btn btn-link" v-on:click="selectedCrisis = ''" aria-label="Clear this filter">Crisis <span class="material-icons">
cancel
</span></button></li>
                <li class="list-inline-item"  v-if="selectedOpioid"><button class="btn btn-link" v-on:click="selectedOpioid = ''" aria-label="Clear this filter">Opioid Treatment <span class="material-icons">
cancel
</span></button></li>
                <li class="list-inline-item"  v-if="selectedDetox"><button class="btn btn-link" v-on:click="selectedDetox= ''" aria-label="Clear this filter">Detox <span class="material-icons">
cancel
</span></button></li>
                <li class="list-inline-item"  v-if="selectedHousing"><button class="btn btn-link" v-on:click="selectedHousing = ''" aria-label="Clear this filter">Housing <span class="material-icons">
cancel
</span></button></li>
                <li class="list-inline-item"  v-if="selectedSudres"><button class="btn btn-link" v-on:click="selectedSudres = ''" aria-label="Clear this filter">Residential Substance Use Treatment <span class="material-icons">
cancel
</span></button></li>
                <li class="list-inline-item"  v-if="selectedMhres"><button class="btn btn-link" v-on:click="selectedMhres= ''" aria-label="Clear this filter">Residential Mental Health Treatment <span class="material-icons">
cancel
</span></button></li>
                <li class="list-inline-item"  v-if="selectedInvoluntary"><button class="btn btn-link" v-on:click="selectedInvoluntary = ''" aria-label="Clear this filter">Involuntary Commmitment <span class="material-icons">
cancel
</span></button></li>
                <li class="list-inline-item"  v-if="selectedLocations"><button class="btn btn-link" v-on:click="selectedLocations = ''" aria-label="Clear this filter">{{this.selectedLocations}} <span class="material-icons">
cancel
</span></button></li>
                <li class="list-inline-item"  v-if="searchText"><button class="btn btn-link" v-on:click="searchText = ''" aria-label="Clear this filter">{{this.searchText}} <span class="material-icons">
cancel
</span></button></li>
                <li class="list-inline-item"  v-if="selectedYouth"><button class="btn btn-link" v-on:click="selectedYouth = ''" aria-label="Clear this filter">Youth <span class="material-icons">
cancel
</span></button></li>
                <li class="list-inline-item"  v-if="selectedAdults"><button class="btn btn-link" v-on:click="selectedAdults = ''" aria-label="Clear this filter">Adults <span class="material-icons">
cancel
</span></button></li>
                <li class="list-inline-item"  v-if="selectedFamilies"><button class="btn btn-link" v-on:click="selectedFamilies = ''" aria-label="Clear this filter">Families <span class="material-icons">
cancel
</span></button></li>
                <li class="list-inline-item"  v-if="selectedOlder"><button class="btn btn-link" v-on:click="selectedOlder = ''" aria-label="Clear this filter">Older Adults <span class="material-icons">
cancel
</span></button></li>

              </ul>
            </div>
          </div>
        </div>
       



        <!--PROVIDER FILTER END -->
        <section id="errorMessage" v-if="errored">

          <p class="card border">We're sorry, we're not able to retrieve this information at the moment, please try back later</p>

        </section>

        <!-- NO RESULTS MESSAGE START -->
        <section id="noResults" v-if="!loading && !computed_items.length && !errored">

          <div class="card border">
              <div  class="card-title">
              <h3>Hmm... we couldn't find any results</h3>
              </div>
         
            <div class="card-body">
              <p>Try refining your search or filters to see if there are any providers that match.</p>
              <button class="btn btn-xs btn-link" v-on:click="resetForm" aria-label="Clear all filters">Clear all filters</button>
          </div>
          </div>
        </section>
        <!-- NO RESULTS MESSAGE END -->

        <!-- CARD LIST START -->
        <section v-else>
          <!--LOADING ICON START -->
          <div class="row">
            <div class="col">
              <div v-if="loading" class="text-center m-a-lg">
                <div class="spinner-border" role="status">
                 <span class="sr-only">Loading...</span>
                </div>
              </div>
            </div>
        </div>

          <!--CARD ROW CONTAINER START -->
          <div v-if="listVisible">
          <div class="row" v-for="i in Math.ceil(computed_items.length / 3)" :key="i">

            <div :key="i" v-for="(items, i) in computed_items.slice((i - 1) * 3, i * 3)" class="col-md-4 mb-4">

              <!--PROVIDER INFO CARD START -->

              <div class="shadow border card h-100">
                  

                <div class="card-body">
                  <h3 class="card-title">{{ items.provider }}</h3>
                  
                  <div class="row">
                    <div class="col-sm-6">
                      <div class="contacts">
                        <p v-if="items.phone"><a :title="items.phone" :href="'tel:' + items.phone">{{items.phone}}</a></p>
                        <p><a v-if="items.website" target="_blank" :title="items.provider" :href="items.website">Website</a></p>
                        <p v-if="items.address_address"><a target="_blank" :href="'https://www.google.com/maps/dir//' + items.address_address + ', ' + items.address_city + ', ' + items.address_state + ' ' + items.address_zip">Get directions</a></p>

                      </div>
                    </div>
                    <div class="col-sm-6">

                      <div class="row">
                        <div class="col-sm-12">
                          <h4 class="" v-if="items.mental_health || items.substance_use || items.opioid || items.detox || items.crisis_services || items.involuntary || items.housing || items.sud_residential || items.mh_residential">Services</h4>
                          <ul class="list-unstyled">
                            <li v-if="items.mental_health">Mental Health</li>
                            <li v-if="items.substance_use">Substance Use</li>
                            <li v-if="items.opioid">Opioid Treatment</li>
                            <li v-if="items.detox">Detox</li>
                            <li v-if="items.crisis_services">Crisis Services</li>
                            <li v-if="items.involuntary">Involuntary Committment</li>
                            <li v-if="items.housing">Housing Assistance</li>
                            <li v-if="items.sud_residential">Residential Substance Use Treatment</li>
                            <li v-if="items.mh_residential">Residential Mental Health Treatment</li>
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
                        <li class="list-inline-item" v-if="items.adults || items.youth || items.older_adults || items.families"><strong>Groups
                                                    served:</strong></li>
                        <li class="list-inline-item" v-if="items.adults">Adults</li>
                        <li class="list-inline-item" v-if="items.youth">Youth</li>
                        <li class="list-inline-item" v-if="items.older_adults">Older adults</li>
                        <li class="list-inline-item" v-if="items.families">Families</li>
                      </ul>

                    </div>
                  </div>
                </div>
              </div>

              <!--PROVIDER CARD END -->

            </div>

          </div>
          </div>
      <!-- CARD ROW CONTAINER END -->

                <!--LOADING ICON END -->
                <div v-if="mapVisible" class="row flexContainer">

                    <div class="col-md-8 col-md-push-4 p-x-0">
                      <!-- MAP START -->
                      
                      <button type="button" id="setLocation" @click="setLocation()" class="findControls btn btn-sm btn-link">Zoom to my location</button>
        
                      <l-map id="map" ref="map" :center="center" :bounds="bounds">
        
                        <l-tile-layer :url="url">
        
                        </l-tile-layer>
        
                        <div :key="i" v-for="(items, i) in computed_items">
        
                          <l-marker @click="selectedMapAddress = items.address_address, selectedMapCoordinates = items.address.coordinates; pinZoom()" v-if="items.address_address" :lat-lng="[items.address.coordinates[1], items.address.coordinates[0]]">
                            
                          </l-marker>
        
                        </div>
        
                      </l-map>
        
                    </div>
        
                    <div class="mapFlex col-md-4 col-md-pull-8 p-x-0">
        
                      <div class="mapLoad p-a" v-if="!selectedMapAddress">
                        <h2 class="text-center">Search, filter or explore the map</h2>
                        <div class="d-flex justify-content-center"><img class="text-center" src="https://unpkg.com/leaflet@1.2.0/dist/images/marker-icon.png"></div>
                        <p>Click or tap a blue pin and details about that provider will appear in this space.</p>
                       
                        <p>To find providers near you, click or tap "Zoom to my location" to center the map to your current location. (NOTE: You'll need to enable location services on your browser or phone for this feature to work.)</p>
                        
                         
                          <p>To narrow the results, use the search bar or click the filters button.</p>
                     
                      </div>
                      <div :key="i" v-for="(items, i) in computed_map" class="providerContainer flex-container">
        
                <div v-if="selectedMapAddress" class="card border h-100">
                  

                <div class="card-body h-100">
                  <h3 class="card-title">{{ items.provider }}</h3>
                  
                  <div class="row">
                    <div class="col-sm-6">
                      <div class="contacts">
                        <p v-if="items.phone"><a :title="items.phone" :href="'tel:' + items.phone">{{items.phone}}</a></p>
                        <p><a v-if="items.website" target="_blank" :title="items.provider" :href="items.website">Website</a></p>
                        <p v-if="items.address_address"><a target="_blank" :href="'https://www.google.com/maps/dir//' + items.address_address + ', ' + items.address_city + ', ' + items.address_state + ' ' + items.address_zip">Get directions</a></p>

                      </div>
                    </div>
                    <div class="col-sm-6">

                      <div class="row">
                        <div class="col-sm-12">
                          <h4 class="" v-if="items.mental_health || items.substance_use || items.opioid || items.detox || items.crisis_services || items.involuntary || items.housing || items.sud_residential || items.mh_residential">Services</h4>
                          <ul class="list-unstyled">
                            <li v-if="items.mental_health">Mental Health</li>
                            <li v-if="items.substance_use">Substance Use</li>
                            <li v-if="items.opioid">Opioid treatment</li>
                            <li v-if="items.detox">Detox</li>
                            <li v-if="items.crisis_services">Crisis Services</li>
                            <li v-if="items.involuntary">Involuntary Committment</li>
                            <li v-if="items.housing">Housing Assistance</li>
                            <li v-if="items.sud_residential">Residential substace use treatment</li>
                            <li v-if="items.mh_residential">Residential mental health treatment</li>
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
                        <li class="list-inline-item" v-if="items.adults || items.youth || items.older_adults || items.families"><strong>Groups served:</strong></li>
                        <li class="list-inline-item" v-if="items.adults">Adults</li>
                        <li class="list-inline-item" v-if="items.youth">Youth</li>
                        <li class="list-inline-item" v-if="items.older_adults">Older adults</li>
                        <li class="list-inline-item" v-if="items.families">Families</li>
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
import { LMap, LTileLayer, LMarker} from 'vue2-leaflet';
import axios from 'axios';
export default {
  name: 'ProvidersLookup',
  components: {
    LMap,
    LTileLayer,
    LMarker
  },
  data() {
    return {
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
      if (event.code === 13) {
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
    resetMap: function() {
      const mapComponent = this.$refs.map.mapObject;
      mapComponent.closePopup();
      this.bounds = LMap.latLngBounds([
        [49.004521, -117.037357],
        [45.645316, -124.588534]
      ]);
    },

    pinZoom: function() {
      const mapComponent = this.$refs.map.mapObject;
      var latLngs = this.selectedMapCoordinates;
      var lats = latLngs[1];
      var longs = latLngs[0];
      var markerBounds = L.latLng([lats, longs]);
      var zoomLev = mapComponent.getZoom();
      if (zoomLev < 12) {
        mapComponent.setView(markerBounds, 12);
      }
      mapComponent.panTo(markerBounds);
    },

    setLocation: function() {
      const mapComponent = this.$refs.map.mapObject;
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(
          function(position) {
            let latlng = L.latLng(
              position.coords.latitude,
              position.coords.longitude
            );
            
            mapComponent.setView(latlng, 14);
            console.log(latlng);

           
           //mapComponent.panTo(latlng)

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
