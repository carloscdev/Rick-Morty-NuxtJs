<template>
  <b-container class="container mt-5">
    <b-row>
      <b-card no-body class="overflow-hidden" style="width: 100%;"
        :footer="'Fecha de Creación : '+personaje.created"
        footer-tag="footer"
        footer-bg-variant="info"
        footer-text-variant="white"
      >
        <b-row no-gutters>
          <b-col md="3">
            <b-card-img :src="personaje.image" :alt="personaje.name" class="rounded-0"></b-card-img>
          </b-col>
          <b-col md="9">
            <b-card-body :title="'N° '+personaje.id+' - '+personaje.name">
              <b-card-text class="mt-5">                
                <p>Estado: {{personaje.status}}</p> 
                <p>Especie: {{personaje.species}}</p>
                <p>Tipo: {{personaje.type}}</p>
              </b-card-text>
            </b-card-body>
          </b-col>
        </b-row>
      </b-card>
    </b-row>
  </b-container>
</template>
<script>
import axios from 'axios'
export default {
  asyncData({params}) {
    return axios.get(`https://rickandmortyapi.com/api/character/${params.id}`).then(res => {
      return {
        personaje: res.data
      
      }       
    })
  }
}
</script>