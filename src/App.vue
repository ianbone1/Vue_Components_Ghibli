<template>
  <div id="app">
    <div v-if="!filmsFetched && !filmsFetched"><h2>Fetching.....Films</h2></div>
    <div v-if="!filmsFetched && !peopleFetched"><h2>Fetching.....People</h2></div>
    <div v-if="!filmsFetched && !speciesFetched"><h2>Fetching.....Species</h2></div>
    <div v-if="!filmsFetched && !locationsFetched"><h2>Fetching.....Locations</h2></div>
    <div v-if="!filmsFetched && !vehiclesFetched"><h2>Fetching.....Vehicles</h2></div>
    <div v-if="filmsFetched">
      <h1>Ghibli!</h1>
      <button type="button" name="resetAll" v-on:click="resetAll">Reset Filters</button>
      <div class="status">
      <div class="row"><div class="label">Films:</div><div class="data">{{filmList.length}}</div></div>
      <div class="row"><div class="label">People:</div><div class="data">{{allPeople.length}}</div></div>
      <div class="row"><div class="label">Species:</div><div class="data">{{allSpecies.length}}</div></div>
      <div class="row"><div class="label">Locations:</div><div class="data">{{allLocations.length}}</div></div>
      <div class="row"><div class="label">Vehicles:</div><div class="data">{{allVehicles.length}}</div></div>
      <div class="row"><div class="label">Cast List:</div><div class="data">{{castList.length}}</div></div>
    </div>
      <div class='appWrapper'>
        <div class='filmListWrapper' v-if="filmListActive">
          <film-list :filmList="filmList"></film-list>
        </div>
        <div class='speciesListWrapper' v-if="speciesListActive">
          <species-list :speciesList="speciesList"></species-list>
        </div>
        <div class="speciesDetailsWrapper" v-if="speciesDetailsActive">
          <species-details  :species="speciesMemberSelected"></species-details>
        </div>
        <div class="filmDetailsWrapper" v-if="filmDetailsActive">
          <film-details  :film="filmSelected"></film-details>
        </div>
        <div class="castWrapper" v-if="filmCastActive" >
          <cast-list :cast="castList"></cast-list>
        </div>
        <div class="castMemberWrapper" v-if="castDetailsActive">
          <cast-details :castMember="castMemberSelected"></cast-details>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import FilmList from "./components/filmList.vue";
import FilmDetails from "./components/filmDetails.vue";
import CastList from "./components/castList.vue";
import CastDetails from "./components/castMemberDetails.vue";
import SpeciesList from "./components/speciesList.vue"
import SpeciesDetails from "./components/speciesDetails.vue"

import { eventBus } from "./main.js";

export default {
  name: 'app',
  data () {
    return {
      filmsFetched: false,
      peopleFetched: false,
      speciesFetched: false,
      locationsFetched: false,
      vehiclesFetched: false,
      filmListActive: false,
      filmDetailsActive: false,
      filmCastActive: false,
      castListActive: false,
      castDetailsActive: false,
      speciesListActive: false,
      speciesDetailsActive: false,
      allFilmsURL:"https://ghibliapi.herokuapp.com/films/",
      allPeopleURL: "https://ghibliapi.herokuapp.com/people/",
      allSpeciesURL: "https://ghibliapi.herokuapp.com/species/",
      allLocationsURL:"https://ghibliapi.herokuapp.com/locations/",
      allVehiclesURL: "https://ghibliapi.herokuapp.com/vehicles/",
      allFilms: [],
      filmList: [],
      allPeople: [],
      allSpecies: [],
      speciesList: [],
      allLocations: [],
      allVehicles: [],
      tempDataStore: [],
      filmSelected: [],
      castMemberSelected: "",
      speciesMemberSelected: "",
      speciesSelected: "",
      filmCast: [],
      castList: []
    }
  },
  methods: {
    generateFilmCastDetails() {
      this.castList=[];
      if (this.filmCast[0] == this.allPeopleURL) {
        this.castList=this.allPeople;
      } else {
        for (let castURL of this.filmCast) {
          let castMember = this.allPeople.find(c=> c.url==castURL);
          this.castList.push(castMember)
        }
      }
    },

    generateFilmList() {
      this.filmList=[];
      this.filmList = this.allFilms.filter(f=> f.people==this.allPeopleURL)
        for (let filmURL of this.castMemberSelected.films) {
          let film = this.allFilms.find(f=> f.url==filmURL);
          this.filmList.push(film)
        }
    },
    resetAll(){
      this.filmList=this.allFilms;
      this.speciesList=this.allSpecies;
      this.castMemberSelected=""
      this.castDetailsActive=false;
      this.generateFilmCastDetails();
      this.filmDetailsActive=false;
      this.filmCastActive=false;
      this.castListActive=false;
      this.filmListActive=true;
      this.speciesListActive=true;
      this.speciesDetailsActive=false;
    },
    generateSpeciesCastList() {
      this.castList=[];
      for (let memberURL of this.speciesMemberSelected.people){
        let speciesMember=this.allPeople.find(s=> s.url==memberURL)
        this.castList.push(speciesMember)
      }
    }
  },
  mounted() {
    fetch(this.allFilmsURL)
    .then(response => response.json())
    .then(theData => (this.allFilms = theData))
    .then(() => this.filmList = this.allFilms)
    .then(() => this.filmsFetched=true);
    fetch(this.allPeopleURL)
    .then(response => response.json())
    .then(theData => (this.allPeople = theData))
    .then(() => this.peopleFetched=true);
    fetch(this.allSpeciesURL)
    .then(response => response.json())
    .then(theData => (this.allSpecies = theData))
    .then(() => this.speciesList=this.allSpecies)
    .then(() => this.speciesFetched=true)
    .then(() => this.speciesListActive=true);
    fetch(this.allLocationsURL)
    .then(response => response.json())
    .then(theData => (this.allLocations = theData))
    .then(() => this.locationsFetched=true);
    fetch(this.allVehiclesURL)
    .then(response => response.json())
    .then(theData => (this.allVehicles = theData))
    .then(() => this.vehiclesFetched=true)
    .then(() => this.filmListActive = true);

    // this.setFilmsFetched();
//species-list
    eventBus.$on("film-selected", film => {
      this.filmSelected = film;
      this.filmDetailsActive=true;
      this.filmCastActive=false;
      this.castListActive=false;
      this.filmListActive=true;
      this.speciesListActive=false;
      this.speciesDetailsActive=false;
    });
    eventBus.$on("film-cast", film => {
      this.filmCast = film.people;
      this.castMemberSelected=""
      this.castDetailsActive=false;
      this.generateFilmCastDetails();
      this.filmDetailsActive=true;
      this.filmCastActive=true;
      this.castListActive=false;
      this.filmListActive=true;
    });
    eventBus.$on("castMember-selected", castMember => {
      this.castMemberSelected = castMember;
      this.filmDetailsActive=false;
      this.filmCastActive=true;
      this.castListActive=true;
      this.castDetailsActive=true;
      this.filmListActive=false;
      this.speciesListActive=false;
    });
    eventBus.$on("film-list", castMember => {
      this.castMemberSelected = castMember;
      this.generateFilmList();
      this.filmListActive=true;
      this.filmDetailsActive=false;
      this.filmCastActive=false;
      this.castListActive=false;
      this.castDetailsActive=false;
    });
    eventBus.$on("speciesMember-selected", speciesMember => {
      this.speciesMemberSelected = speciesMember;
      console.log(this.speciesMemberSelected.name)
      // this.generateFilmList();
      this.filmListActive=false;
      this.filmDetailsActive=false;
      this.filmCastActive=false;
      this.castListActive=false;
      this.castDetailsActive=false;
      this.speciesListActive=true;
      this.speciesDetailsActive=true;
      this.filmDetailsActive=false
    });

    eventBus.$on("species-members-list", speciesMember => {
      this.speciesMemberSelected = speciesMember;
      this.generateSpeciesCastList();
      this.filmListActive=false;
      this.filmDetailsActive=false;
      this.filmCastActive=true;
      this.castListActive=true;
      this.castDetailsActive=false;
      this.speciesListActive=true;
      this.speciesDetailsActive=true;
    });

    eventBus.$on("species-list", castMember =>{
      let speciesURL=castMember.url;
      let species=this.allSpecies.find(s=>s.url=speciesURL);
      eventBus.$emit("species-members-list", species);
    });


    eventBus.$on("species-films-list", speciesMember => {
      this.speciesMemberSelected = speciesMember;
      console.log(this.speciesMemberSelected.name)
      // // this.generateFilmList();
      // this.filmListActive=false;
      // this.filmDetailsActive=false;
      // this.filmCastActive=false;
      // this.castListActive=false;
      // this.castDetailsActive=false;
      // this.speciesListActive=true;
      // this.speciesDetailsActive=true;
    });

  },
  components: {
    "film-list": FilmList,
    "film-details": FilmDetails,
    "cast-list": CastList,
    "cast-details": CastDetails,
    "species-list": SpeciesList,
    "species-details": SpeciesDetails
  }
}
</script>


<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  .status{
    display:block;
    flex-direction:column;
    padding:0;
    margin:0;
  }
  .row{
    display: flex;
    justify-content: center;
  }
  .label{
    display:flex;
    align-items: flex-end;
    justify-content: flex-end;
    width: 40%;
  }
  .data{
    width:40%;
    display: flex;
    align-items: flex-start;
  }

  .appWrapper {
    width: 100%;
    display: flex;
    justify-content:space-around;
  }
  .filmListWrapper{
    width: 30%;
  }
  .filmDetailsWrapper{
    width: 30%;
  }
  .speciesListWrapper{
    width: 30%;
  }
  .speciesDetailsWrapper{
    width: 30%;
  }
  .castWrapper{
    width: 30%;
  }
  .castMemberWrapper{
    width: 30%;
  }
  h1, h2, h3{
    font-weight: normal;
  }

  ul {
    list-style-type: none;
    padding: 0;
  }

  li {
    margin: 0 10px;
  }

  a {
    /* color: #42b983; */
  }
  a:hover{
    color: red;
  }
  button:hover{
    color:red;
  }
</style>
