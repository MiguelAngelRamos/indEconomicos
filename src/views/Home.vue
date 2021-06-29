<template>
  <Layout>
    <div class="form-group">
      <label><h3>Selecciona un Indicador</h3></label>
      <!--Select -->
      <select v-model="indicador" class="form-select" name="indicador">
        <option v-for="(ind, index) in indicadoresEconomicos" :key="index">
          {{ ind.codigo }}
        </option>
      </select>
      <!-- Select -->
      <Spinner v-if="loading"/>
      <!-- Indicadores info -->
      <div class="mt-4" v-if="indicadorInfo.serie">
          <h3> Indicador: {{ indicador }}</h3>
        <div class="indicadores-info mt-4">
          <h4>Indicador: {{ indicadorInfo.nombre}}</h4>
          <h5>Unidad de Medida: {{ indicadorInfo.unidad_medida}}</h5>
        </div>
      </div>
   
      <!-- /Indicadores info -->
      <!-- Tabla -->
      <table v-if="indicadorInfo.serie" class="table mt-5">
        <thead>
          <tr>
            <th scope="col">Fecha</th>
            <th scope="col">Valor</th>
          </tr>
        </thead>
        <tbody>

          <tr v-for="ind in indicadorInfo.serie" :key="ind.fecha">
            <td>{{ ind.fecha }}</td>
            <td>{{ ind.valor }}</td>
          </tr>

        </tbody>
      </table>
      <!-- /Tabla -->
    </div>
  </Layout>
</template>

<script>
// @ is an alias to /src
import Layout from "../layout/Layout.vue";
import Spinner from "../components/Spinner.vue";
import { indicadoresEconomicos } from "../api/indicadores";

export default {
  name: "Home",
  components: {
    Layout,
    Spinner
  },
  data() {
    return {
      indicadorInfo: [],
      indicadoresEconomicos: [
        { codigo: 'uf'},
        { codigo: 'ivp'},
        { codigo: 'dolar'},
        { codigo: 'dolar_intercambio'},
        { codigo: 'euro'},
        { codigo: 'ipc'},
        { codigo: 'utm'},
        { codigo: 'imacec'},
        { codigo: 'tpm'},
        { codigo: 'libra_cobre'},
        { codigo: 'tasa_desempleo'},
        { codigo: 'bitcoin'},
      ],
      indicador: '',
      loading: false
    }
  },
  methods: {
    async getIndicadores() {
      try {
        console.log('invocando get indicadores');
        this.indicadorInfo = await indicadoresEconomicos(this.indicador);
        console.log(this.indicadorInfo.serie);
        this.loading = false;
        
      } catch (error) {
        console.log(error);
      }
    }
  },
  created() {
    this.getIndicadores();
  },
  watch: {
    /* Recibe por parametro el nuevoValor, el valor anterior*/
    indicador() {
      this.loading = true;
      this.indicadorInfo = [];
      console.log("estoy en el watch");
      this.getIndicadores();
    }
  }
};
</script>
