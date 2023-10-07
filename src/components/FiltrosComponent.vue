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
        :cidade="filtros.cidade"
        :selecionadas="filtrosCategorias.cidade"
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
      {{ filtrosCategorias.cidade }}
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
  props: ["filtros", "filtrosSelecionados"],
  components: {
    FiltroEstadosCidades,
    FiltroValor,
  },
  data: () => ({
    filtrosCategorias: {
      cidade: [],
      tipoImovel: [],
      quartos: [],
      averbacao_leiloes: [],
      condicoes: [],
    },
    filtrosValores: {},
  }),
  created: function () {
    if (
      this.filtrosSelecionados !== null &&
      typeof this.filtrosSelecionados !== "undefined"
    ) {
      //////////////////////////// TRAVADO AQUI ////////////////////////////
      console.log("beforeCreate", this.filtrosSelecionados);
      console.log(this);
      if ("filtrosCategorias" in this.filtrosSelecionados) {
        var filtrosCategoriasArr = {};
        for (var [key, value] of Object.entries(this.filtrosSelecionados.filtrosCategorias)) {
          filtrosCategoriasArr[key] = value.map(valor => ({ val: valor }));
        }
        this.filtrosCategorias = filtrosCategoriasArr;        
      }
      if ("filtrosValores" in this.filtrosSelecionados) {
        //this.filtrosValores = this.filtrosSelecionados.filtrosValores;
      }
    }
  },
  methods: {
    //captura a informação das cidades selecionadas do componente filtros
    update_cidades_selecionadas: function (params) {
      console.log("Recebi o evento update_cidades_selecionadas", params);
      this.filtrosCategorias.cidade = params;
      console.log(this);
    },
  },
  watch: {
    filtrosCategorias: {
      handler(FiltrosCat) {
        console.log(
          "FiltrosComponent Filtro de Categorias Atualizado",
          FiltrosCat
        );

        var filtrosCategoriasArr = [];

        for (const [key, value] of Object.entries(FiltrosCat)) {
          console.log(key, value);
          if (value.length > 0) {
            const valoresSelecionados = value.map((item) => item.val);

            const filtroEnvelope = {
              nomeFiltro: key,
              tipoFiltro: "filtrosCategorias",
              valoresSelecionados: valoresSelecionados,
            };
            
            console.log("update_filtros", filtroEnvelope);
            filtrosCategoriasArr.push(filtroEnvelope);
            //this.filtrosSelecionados[key] = value;
          } else {
            //delete this.filtrosSelecionados[key];
          }
        }
        console.log(
          "FiltrosComponent update_filtros_categoria",
          filtrosCategoriasArr
        );
        this.$emit("update_filtros_categoria", filtrosCategoriasArr);

        /*
        const valoresSelecionados = valores.map((item) => item.val);
        const filtroEnvelope = {
          nomeFiltro: "cidade",
          tipoFiltro: "filtrosCategoria",
          valoresSelecionados: valoresSelecionados,
        };
        this.$emit("update_filtros", filtroEnvelope);
        */
      },
      deep: true,
    },
  },
};
</script>
