<template>
  <v-container fluid>
    <v-row dense>
      <v-col
        class="imovel"
        v-for="imovel in imoveis"
        :key="imovel.id_imovel"
        :cols="3"
      >
        <v-card>
          <v-img
            :src="imovel.src"
            class="white--text align-end"
            gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)"
            height="200px"
          >
            <v-card-title v-if="imovel.valor_de_venda">
              {{ formataReais(imovel.valor_de_venda) }}
            </v-card-title>
            <v-card-title v-if="imovel.valor_minimo_de_venda">
              {{ formataReais(imovel.valor_minimo_de_venda) }}
              <v-chip class="ma-2" x-small> mínimo </v-chip>
            </v-card-title>
            <v-card-title v-if="imovel.valor_minimo_de_venda_a_vista">
              {{ formataReais(imovel.valor_minimo_de_venda_a_vista) }}
              <v-chip class="ma-2" x-small> a vista </v-chip>
            </v-card-title>
          </v-img>

          <v-card-subtitle>
            ... {{ imovel.descricao.slice(-110) }}</v-card-subtitle
          >

          <v-card-text>
            <!-- Áreas -->
            <v-row dense>
              <v-col cols="4" v-if="imovel.area_privativa">
                <span>Área Privativa</span>
                <v-card-subtitle>
                  {{ imovel.area_privativa[0] }}
                  {{ imovel.area_privativa_unidade }}
                </v-card-subtitle>
              </v-col>
              <v-col cols="4" v-if="imovel.area_total">
                <span>Área Total</span>
                <v-card-subtitle>
                  {{ imovel.area_total }}
                  {{ imovel.area_total_unidade }}
                </v-card-subtitle>
              </v-col>
              <v-col cols="4" v-if="imovel.terreno">
                <span>Área Total</span>
                <v-card-subtitle>
                  {{ imovel.terreno }} {{ imovel.terreno_unidade }}
                </v-card-subtitle>
              </v-col>
            </v-row>

            <v-row dense>
              <v-col cols="4" v-if="imovel.tipo_de_imovel">
                <span>Tipo</span>
                <v-card-subtitle>
                  {{ imovel.tipo_de_imovel }}
                </v-card-subtitle>
              </v-col>
              <v-col cols="4" v-if="imovel.quartos">
                <span>Quartos</span>
                <v-card-subtitle>
                  {{ imovel.quartos }}
                </v-card-subtitle>
              </v-col>
              <v-col cols="4" v-if="imovel.garagem">
                <span>Garagem</span>
                <v-card-subtitle>
                  {{ imovel.garagem }}
                </v-card-subtitle>
              </v-col>
            </v-row>

            <v-card-subtitle>
              {{ imovel.endereco.slice(0, 110) }}
            </v-card-subtitle>
          </v-card-text>

          <v-card-actions>
            <v-btn 
                color="orange" 
                text
                :href="`https://venda-imoveis.caixa.gov.br/sistema/detalhe-imovel.asp?hdnOrigem=index&hdnimovel=${imovel.id_imovel}`"
                > Visitar </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<style>
.imovel .v-card__subtitle {
  font-size: 0.7rem;
  padding: 0;
}

.imovel .v-card > .v-card__subtitle {
  /* Estilos para o primeiro .v-card__subtitle */
  min-height: 66px;
}

.imovel span {
  font-size: 0.8rem;
}
</style>

<script>
export default {
  name: "ImoveisLista",
  props: ["imoveis"],
  methods: {
    formataReais(valor) {
      //Verifica se o valor é um número
      if (isNaN(valor)) {
        return valor;
      }
      return valor.toLocaleString("pt-BR", {
        style: "currency",
        currency: "BRL",
      });
    },
  },
};
</script>
