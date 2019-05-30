<template>
  <div id="app">
    <Navbar />
    <!-- Container -->
    <div class="container-fluid">
      <div class="row">
        <div class="col-12">
          <Map :locations="cdmxStores" v-if="cdmxStores.length > 0" />
        </div>
      </div>
      <div class="row mb-2">
        <div class="col-md-4 col-xs-12">
          <Table class="shadow-sm" :fields="versions_fields" :items="versions" :title="versions_title" />
        </div>
        <div class="col-md-4 col-xs-12">
          <Table class="shadow-sm" :items="devices" :title="devices_title" />
        </div>
        <div class="col-md-4 col-xs-12">
          <Table class="shadow-sm" :items="systems" :title="systems_title" />
        </div>
      </div>
      <div class="row mb-2">
        <div class="col-12">
          <Table class="shadow-sm" :items="issues" :title="issues_title" />
        </div>
      </div>
      <div class="row mb-2">
        <div class="col-4 col-sm-6">
          <switches v-model="categoriesSwitch" theme="bootstrap" color="danger"></switches>
          <p>Switch component</p>
          <Table class="shadow-sm" v-if="categoriesSwitch" :items="categories" title="Google Play Store Apps by Category" />
          <div v-else>
            <BarChart v-if="labelsCategories.length > 0 && dataCategories.length > 0 " canvasID="categories" :labels="labelsCategories" :data="dataCategories" />    
          </div>
        </div>
        <div class="col-4 col-sm-6">
          <switches v-model="ratingsSwitch" theme="bootstrap" color="danger"></switches>
          <p>Switch component</p>
          <Table class="shadow-sm" v-if="ratingsSwitch" :items="rating" title="Google Play Store Apps by Rating" />
          <div v-else>
            <BarChart  v-if="labelsRating.length > 0 && dataRating.length > 0 " canvasID="rating" :labels="labelsRating" :data="dataRating" />    
          </div>
        </div>
        <div class="col-4 col-sm-6">
          <switches v-model="pricesSwitch" theme="bootstrap" color="danger"></switches>
          <p>Switch component</p>
          <Table class="shadow-sm" v-if="pricesSwitch" :items="prices" title="Google Play Store Apps by Price" />
          <div v-else>
            <BarChart  v-if="labelsPrices.length > 0 && dataPrices.length > 0 " canvasID="prices" :labels="labelsPrices" :data="dataPrices" />        
          </div>
        </div>
      </div>
      <div class="row mb-2">
        <div class="col-12">
          <Table class="shadow-sm" :items="data" title="Google Play Store Apps" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import Table from "@/components/Table.vue";
import BarChart from "@/components/BarChart.vue";
import * as d3 from 'd3';
import Switches from 'vue-switches';
import Map from '@/components/Map.vue';  

export default {
  name: "App",
  components: {
    Navbar,
    Table,
    Switches,
    BarChart,
    Map
  },
  data() {
    return {
      cdmxStores: [],
      labelsCategories: [],
      dataCategories: [],
      labelsRating: [],
      dataRating: [],
      labelsPrices: [],
      dataPrices: [],
      categoriesSwitch: false,
      ratingsSwitch: false,
      pricesSwitch: false,
      data: [],
      categories: [],
      rating: [],
      prices: [],
      versions_title: "Crashes by version",
      versions_fields: [
        { key: 'version', sortable: false },
        { key: 'events', sortable: true },
        { key: 'users', sortable: false }
      ],
      versions: [
        { version: 2.64, events: '4,931', users: '4,013' },
        { version: 2.33, events: '3,999', users: '2,002' },
        { version: 2.43, events: '3,999', users: '1,451' },
        { version: 2.53, events: '3,999', users: '5,546' },
        { version: 2.62, events: '1,006', users: '970' }
      ],
      devices_title: "Crashes by device",
      devices: [
        { 'device model': 'ONEPLUS A3003', events: '3,596', users: '3,088' },
        { 'device model': 'Mi A1', events: '2,797', users: '2,485' },
        { 'device model': 'Pixel 2 XL', events: '1,961', users: '1,823' },
        { 'device model': 'Pixel 2', events: '1,184', users: '1,119' },
        { 'device model': 'SM-G950F', events: '398', users: '391' }
      ],
      systems_title: "Crashes by operating system",
      systems: [
        { 'OS': '9', events: '3,568', users: '3,085' },
        { 'OS': '7.1.1', events: '2,782', users: '2,447' },
        { 'OS': '8.1.0', events: '2,004', users: '1,844' },
        { 'OS': '7.0', events: '1,156', users: '1,098' },
        { 'OS': '8.0.0', events: '426', users: '420' }
      ],
      issues_title: "Issue list",
      issues: [
        { issue: 'ProgressiveMode.java - line 20', 'blame frame': 'com.labpixies.flood.ProgressiveMode.getLevel', version: '2.64', events: '4,016', users: '3,387' },
        { issue: 'AdHelper.java - line 111', 'blame frame': 'com.labpixies.flood.AdHelper.populateAppInstallAdView', version: '2.63', events: '3,900', users: '3,289' },
        { issue: 'AdHelper.java - line 150', 'blame frame': 'com.labpixies.flood.AdHelper.populateAppInstallAdView', version: '2.64', events: '915', users: '882' },
        { issue: 'ExtraStepsActivity.java - line 145', 'blame frame': 'com.labpixies.flood.ExtraStepsActivity.setUpPackage', version: '2.62', events: '791', users: '769' },
        { issue: 'ProgressiveMode.java - line 20', 'blame frame': 'com.labpixies.flood.ProgressiveMode.getLevel', version: '2.62', events: '215', users: '214' },
        { issue: 'FloodItActivity.java - line 662', 'blame frame': 'com.labpixies.flood.FloodItActivity.shouldShowInterstitialAd', version: '2.63', events: '99', users: '99' }
      ]
    }
  },
  methods: {
    getStarbucksStores() {
      d3.csv('/assets/directory.csv').then(stores => {
        // console.log(stores);
        const cdmxStores = stores.filter(s => s.City === "Mexico City");
        // console.log(cdmxStores)
        this.cdmxStores = cdmxStores;
      });
    }
  },
  mounted() {
    this.getStarbucksStores();

    d3.csv('/assets/googleplaystore.csv', function(d) {
        if (d.Rating === 'Free') {
          d.Rating = 0;
        }

        return d;
      }).then(apps => {
      //console.log(apps);
      //this.data = apps;


      ////////////////////////////////////////

      //////////////////////////////////////// 
      const categories = d3.nest()
        .key(d => d.Category)
        .rollup(d => d.length)
        .entries(apps);

      /*categories.forEach(c => {
        c.Category = c.key;
        c.App = c.value;
      });*/

      this.categories = categories;
      //console.log(categories);
      this.categories.sort((a,b) => b.value - a.value);
      this.labelsCategories = this.categories.map(c => c.key);
      //console.log(labelsCategories);
      this.dataCategories = this.categories.map(c => c.value);
      //console.log(dataCategories);
      //////////////////////////////////////// 
      const rating = d3.nest()
        .key(d => d.Rating)
        .rollup(d => d.length)
        .entries(apps);

      this.rating = rating;
      //console.log(rating);
      this.rating.sort((a,b) => b.value - a.value);
      this.labelsRating = this.rating.map(c => c.key);
      //console.log(labelsRating);
      this.dataRating = this.rating.map(c => c.value);
      //console.log(dataRating);

      //////////////////////////////////////// 
      const prices = d3.nest()
        .key(d => d.Price)
        .rollup(d => d.length)
        .entries(apps);

      this.prices = prices;
      //console.log(prices);
      this.prices.sort((a,b) => b.value - a.value);
      this.labelsPrices = this.prices.map(c => c.key);
      //console.log(labelsPrices);
      this.dataPrices = this.prices.map(c => c.value);
      //console.log(dataPrices);
    });
  }
};
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Roboto');
#app {
  background-color: #F0F0F0;
}
</style>
