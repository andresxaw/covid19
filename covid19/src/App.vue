<template lang="pug">
  #app.app
    img.line.img-colombia(src="/static/colombia.png")
    h2.title.line COVID-19 - COLOMBIA
    div
      h2.title.line #yomequedoencasa
    div(v-if="casesColombia")        
      .item Casos Totales: 
        .number {{ casesColombia.cases }}
      .item Hoy: 
        .number {{ casesColombia.todayCases }}
      .item Muertes: 
        .number {{ casesColombia.deaths }}
      .item Muertes Hoy: 
        .number {{ casesColombia.todayDeaths }}
      .item Recuperados: 
        .number {{ casesColombia.recovered }}
      .item Casos Activos: 
        .number {{ casesColombia.active }}
      .item En Estado Crítico: 
        .number {{ casesColombia.critical }}
    div(v-else)
      .error {{ error }}
    div
      a(href="https://sahet.co")
        img.logo(src="/static/logo.png")
      .info En esta página puedes ver el avance del COVID19 (Corona Virus) en tiempo real 

</template>

<script>
const axios = require("axios").default;

export default {
  name: "app",
  data() {
    return {
      casesColombia: '',
      error: ''
    }
  },

  async mounted() {
    try {
      const response = await axios.get("https://coronavirus-19-api.herokuapp.com/countries")
      
      if(response && response.data.length > 0){
        const data = response.data

        for (let i = 0; i < data.length; i++) {
          const caseC = data[i]
          
          if(caseC.country === 'Colombia'){
            this.casesColombia = caseC
            break
          } 
        }
      }
    } catch (error) {
      this.error = 'Intenta más tarde. No pudimos obtener los datos :('
    }
  }
}

</script>

<style lang="stylus">
.app 
  font-family Raleway, serif
  text-align center

.logo
  height 1.5em
  margin 1em

.title 
  font-size 1.7em
  margin .3em 0

.line
  display inline-block
  vertical-align middle

.img-colombia
  height 2.5em
  margin-right 1em

.item
  font-weight 600
  text-transform uppercase
  margin .7em
  font-size .8em

.number
  font-size 1.1em

.info
  font-weight 600
  padding 0 1em
  font-size .9em

.error
  color red
  margin 1em 0
  font-weight 600
</style>
