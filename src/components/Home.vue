<template>
  <div
    class="ion-page"
    main
  >
    <ion-header>
      <ion-toolbar color="primary">
        <ion-buttons slot="start">
          <ion-menu-toggle>
            <ion-button>
              <ion-icon
                slot="icon-only"
                name="menu"
              ></ion-icon>
            </ion-button>
          </ion-menu-toggle>
        </ion-buttons>
        <ion-title>Ionic4 VueJS: Food Truck Saver</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-content padding>
      <ion-row>
        <ion-item style="width:100%">
          <ion-input
            v-validate="'email|required'"
            data-vv-as="email"
            @input="credentials.email = $event.target.value"
            :value="credentials.email"
            name="username"
            type="text"
            spellcheck="false"
            placeholder="enter truck name of food type"
            autocapitalize="off"
          >
          </ion-input>
        </ion-item>
      </ion-row>
      <ion-row>
        <ion-card style='height: 300px; width:100%;padding: 0px'>
          <ion-card-content
            item-content
            style='padding: 0px'
          >
            <div
              id='map'
              style='height: 300px; width:100%; padding: 0px'
            ></div>
          </ion-card-content>
        </ion-card>
      </ion-row>


      <ion-list>
        <ion-item
          v-for="item in allTrucks"
          :key="item.id"
          @click="showTruckDetail(item)"
        >
          <ion-label>
            <h2 v-html="item.c2[0].value"></h2>
            <h3>{{ item.c4[0].value }}</h3>
            <h3>{{ item.c3[0].value }}</h3>
          </ion-label>
        </ion-item>
      </ion-list>
    </ion-content>
  </div>
</template>

<script>

  import { mapActions, mapGetters } from "vuex";

  export default {
    name: "Home",
    props: {
      msg: String
    },
    computed: {
      ...mapGetters("truck", ["authError", "allTrucks"])
    },
    data() {
      return {
        credentials: {}
        // items: this.$state.truck.trucks
      };
    },
    methods: {
      // get actions and getters from vuex state model
      ...mapActions("truck", ["fetchData", "setCurrentTruck"]),
      showTruckDetail(_item) {
        this.setCurrentTruck(_item);
        this.$router.push("home-detail");
      }
    },
    // LIFECYCLE FUNCTIONS

    created() {
      this.$store.dispatch("truck/fetchData");
    },
    mounted: async function() {
      mapboxgl.accessToken =
        "pk.eyJ1IjoiYWFyb25rc2F1bmRlcnMiLCJhIjoiY2pxaXhyZjVrMTFoODQ5bDJ0eG96MHFmayJ9.Lgkvv2h6iVQAOBWIXTHhGQ";
      var map = new mapboxgl.Map({
        container: "map",
        style: "mapbox://styles/mapbox/streets-v9",
        center: [-96, 37.8], // starting position
        zoom: 3 // starting zoom
      });

      map.addControl(
        new mapboxgl.GeolocateControl({
          positionOptions: {
            enableHighAccuracy: true
          },
          trackUserLocation: true
        })
      );
    }
  };
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
