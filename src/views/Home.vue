<template>
<div>
  <v-layout :wrap="true">
    <v-flex xs12>
      <v-card>
        <v-date-picker
        full-width
        locale="es-cl"
        :min="minimo"
        :max="maximo"
        @change="getdolar(fecha)"
        v-model="fecha">
        </v-date-picker>
      </v-card>
      <v-card color="error" dark>
        <v-card-text class="display-1 text-center">
          {{valor}} - {{fecha}} 
        </v-card-text>
      </v-card>
    </v-flex>
   
  </v-layout>
  </div>
</template>
<script>
import  axios  from "axios";
export default {
  data(){
    return{     
      fecha: new Date().toISOString().substr(0, 10),   
      minimo: '1984' ,
      maximo: new Date().toISOString().substr(0, 10),
      valor: null
    }
  },
  methods:{
    async getdolar(dia) {
      let arrayFecha = dia.split ('-')     
      let ddmmaa = arrayFecha[2]+'-'+arrayFecha[1]+'-'+arrayFecha[0];

      try {
        let datos = await axios.get(`https://mindicador.cl/api/dolar/${ddmmaa}`)

        if (datos.data.serie.length > 0) {
          this.valor = await datos.data.serie[0].valor
        } else {
          this.valor='Sin resultados'
        }
        
        
        
      } catch (error) {
        console.log(error)
      } 
      finally {

      }
      
      
    }  
  }, 
  created (){
    this.getdolar(this.fecha)
  }
}
</script>
