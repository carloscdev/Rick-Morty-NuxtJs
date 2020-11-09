<template>
  <b-container class="container mt-5">
    <b-row>
      <b-col cols lg="8" sm="12">
        <b-input
        type="text"
        id="searchText"
        v-model="searchName"
        placeholder="Buscar por nombre"
        required>
      </b-input>
      </b-col>
      <b-col cols lg="4" sm="12">
      <b-button :disabled="searchName == ''" @click="Search()" variant="info" class="my-2 my-sm-0 mr-3">Buscar</b-button>
      <b-button @click="Clear()" variant="outline-info" class="my-2 my-sm-0 mr-3">Mostrar todo</b-button>      
      </b-col>
    </b-row>
    <b-row class="mt-5"> 
      <b-col cols="12" v-if="searchLoading == false">
        <b-button v-if="paginas.prev != null" @click="FirstPage()" variant="outline-info" class="my-0 my-sm-0">Primera Página</b-button>
        <small v-if="paginas.prev != null">-</small>
        <b-button v-if="paginas.prev != null" @click="PrevPage()" variant="outline-info" class="my-0 my-sm-0">Página {{this.pageNumber - 1}}</b-button>
        <small v-if="paginas.prev != null && paginas.next != null ">-</small>
        <b-button v-if="paginas.next != null" @click="NextPage()" variant="outline-info" class="my-0 my-sm-0">Página {{this.pageNumber + 1}}</b-button>
        <small v-if="paginas.next != null">-</small>
        <b-button v-if="paginas.next != null" @click="LastPage()" variant="outline-info" class="my-0 my-sm-0">Última Página</b-button>
      </b-col>
      <b-col class="text-center">
        <div class="overflow-auto">
          <!-- <b-pagination-nav variant="info" v-if="searchLoading == false" @change="Prueba" last-number :link-gen="linkGen" :number-of-pages="paginas.pages" use-router></b-pagination-nav> -->
        </div>
      </b-col>
    </b-row>
    <div class="text-center">
       <b-spinner v-if="spinnerLoading == true"></b-spinner>
    </div>
    <b-row class="mt-5" >
      <b-col cols lg="3" md="6" sm="12" col v-for="personaje in personajes" :key="personaje.id">
              <b-card
              :title="personaje.name"
              :img-src="personaje.image"
              img-alt="Image"
              img-top
              tag="article"
              class="mb-2"
            >
              <b-card-text>
                <small >{{personaje.id}} | {{personaje.status}} | {{personaje.species}}</small>
              </b-card-text>

              <nuxt-link class="btn btn-info" :to="'/personajes/' + personaje.id">Ver Detalles</nuxt-link>
            </b-card>

      </b-col>
    </b-row>
  </b-container>
</template>

<script>
import axios from 'axios'
var pageNumber = 1
export default {
  data() {
    return {
      personajes: [],
      searchName: '',    
      searchLoading: false,
      spinnerLoading: false,
      pageNumber: 1
    }
  },
  asyncData(){         
    return axios.get('https://rickandmortyapi.com/api/character/', {
      params: {
          page: pageNumber
        }
    }).then(res => {
      return {
        personajes: res.data.results,
        paginas: res.data.info,
      }
    })
  },
  methods: {
    async Search() {
      this.spinnerLoading = true
      this.personajes = []     
      this.searchLoading = true
      return axios.get('https://rickandmortyapi.com/api/character/', {
        params: {
          name: this.searchName
        }
      }).then(res => {
        setTimeout(() => {
          this.spinnerLoading = false
          this.personajes = res.data.results
        }, 100)
      })
    },
    async Clear() {
      this.searchName = ""
      this.searchLoading = false
      pageNumber = 1
      this.pageNumber = 1
      this.General()
    },    
    async NextPage() {
      pageNumber += 1
      this.pageNumber += 1
      this.General()
    },
    async PrevPage() {
      pageNumber -= 1
      this.pageNumber -= 1
      this.General()
    },
    async LastPage() {
      pageNumber = 34
      this.pageNumber = 34
      this.General()
    },
    FirstPage (){
      pageNumber = 1
      this.pageNumber = 1
      this.General()
    },
    async General(){
      this.personajes = []  
      return axios.get('https://rickandmortyapi.com/api/character/', {
        params: {
          page: pageNumber
        }
        }).then(res => {
          setTimeout(() => {
          this.spinnerLoading = false
          this.personajes = res.data.results
          this.paginas = res.data.info
          }, 100)
      })
    },

    /* linkGen(pageNumber) {
        return pageNumber === 1 ? '?' : `?page=${pageNumber}`
    },
    async Prueba(pageNumber){
     
      this.personajes = [] 
      return axios.get('https://rickandmortyapi.com/api/character/', {
        params: {
          page: pageNumber
        }
      }).then(res => {
        this.personajes = res.data.results
        this.paginas = res.data.info
      })
    } */
  },
}
</script>

<style>
</style>
