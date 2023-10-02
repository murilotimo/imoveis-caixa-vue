<template>
  <div>
    <v-expansion-panels
      :accordion="true"
      :flat="false"
      :inset="false"
      :multiple="true"
      :popout="false"
    >
      <!-- Filtro de Estados e Cidades-->
      <FiltroEstadosCidades
        :estados="filtros.estados"
        :cidades_selecionadas="cidades_selecionadas"
        @update_cidades_selecionadas="update_cidades_selecionadas"
      >
      </FiltroEstadosCidades>

      <!-- Filtro Valor -->
      <FiltroValor :valor="filtros.valor"></FiltroValor>

      <!-- Filtros de Categorias -->
      <v-expansion-panel
        v-for="(filtro, filtro_idx) in filtros"
        :key="filtro_idx"
      >
        <v-expansion-panel-header>
          {{ filtro_idx }}
        </v-expansion-panel-header>
        <v-expansion-panel-content>
          <!-- Seletores de tipo de imóvel como: terreno, apartamento, casa -->
          <ul>
            <li>Terreno</li>
            <li>Apartamento</li>
            <li>Casa</li>
          </ul>
        </v-expansion-panel-content>
      </v-expansion-panel>
    </v-expansion-panels>

    <div>
      {{ cidades_selecionadas }}
    </div>
    <ol>
      <li v-for="(filtro, filtro_key) in filtros" v-bind:key="filtro_key">
        <p>{{ filtro_key }}</p>
        <p>{{ filtro }}</p>
      </li>
    </ol>
  </div>
</template>

<script>
import FiltroEstadosCidades from "./FiltroEstadosCidades.vue";
import FiltroValor from "./FiltroValor.vue";
//import _ from 'lodash';

export default {
  name: "FiltrosComponent",
  props: ["filtros"],
  components: {
    FiltroEstadosCidades,
    FiltroValor,
  },
  data: () => ({
    cidades_selecionadas: [],
  }),
  methods: {
    //captura a informação das cidades selecionadas do componente filtros
    update_cidades_selecionadas: function (params) {
      console.log("Recebi o evento update_cidades_selecionadas", params);
      console.log(this);
      this.cidades_selecionadas = params;
    },
  },
  watch: {
    cidades_selecionadas: function (valores) {
      console.log("cidades_selecionadas foi atualizado", valores);

      const valoresSelecionados = valores.map(item => item.val);
      const filtroEnvelope = {
        nomeFiltro: "cidade",
        tipoFiltro: "categorico",
        valoresSelecionados: valoresSelecionados
      }

      this.$emit("update_filtros", filtroEnvelope);
    },
  },
};
</script>
