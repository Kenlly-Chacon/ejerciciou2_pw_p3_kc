<template>
  <div>
    <ul>
      <li v-for="state in arreglo" >
        <span>{{state}}</span>
      </li>
    </ul>

    <input v-model="text" v-on:keypress.enter="consumirAPIObtenerIniciales()" type="text" placeholder="Ingrese codigo de estado">

    <div v-if="resultadoBusqueda">
      <label>Casos Positivos: {{ casosPositivos }}</label>
      <label>{{ casosPositivos }}</label>
      <label>Total Resultados Pruebas:</label>
      <label>{{ totalResultadosPruebas }}</label>
      <label>Actualmente Hospitalizados:</label>
      <label>{{ actualmenteHospitalizados }}</label>
      <label>Muertos: </label>
      <label>{{ muertos }}</label>
      <label>Casos Positivos Virales:</label>
      <label>{{ casosPositivosVirales }}</label>
      <label>Numero Aumento Muertes:</label>
      <label>{{ numeroAumentoMuertes }}</label>
    </div>
  </div>
</template>

<script>
export default {
  name: "Covid",
  data() {
    return {
      arreglo: [0,1,2,3],
      text:null,

      casosPositivos: null,
      totalResultadosPruebas: null,
      actualmenteHospitalizados: null,
      muertos: null,
      casosPositivosVirales: null,
      numeroAumentoMuertes: null,

      resultadoBusqueda : false
    }
  },
  methods: {
    async consumirAPI(id) {
      const data = await fetch("https://api.covidtracking.com/v1/states/" + id + "/current.json").then(r => r.json())
      const {positive, totalTestResults, hospitalizedCurrently, death, totalTestsViral, deathIncrease} = data
      this.casosPositivos = positive
      this.totalResultadosPruebas = totalTestResults
      this.actualmenteHospitalizados = hospitalizedCurrently
      this.muertos = death
      this.casosPositivosVirales = totalTestsViral
      this.numeroAumentoMuertes = deathIncrease
    },
    async consumirAPIObtenerIniciales() {
      const arreglos = []
      const date = await fetch("https://api.covidtracking.com/v1/states/current.json").then(r => r.json())
      for(const it in date){
        const valor = date[it]
        const {state} = valor
        arreglos.unshift(state)
        this.arreglo = arreglos
      }

      for(const valor of this.arreglo){
        console.log(valor)
        console.log(this.text)
        if(valor === this.text){
          console.log("************************************************************")
          await this.consumirAPI(valor.toLowerCase())
        }

      }
      this.resultadoBusqueda = true
    }
  }
}
</script>

<style scoped>

</style>