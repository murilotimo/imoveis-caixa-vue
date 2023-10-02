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
            <v-card-title>
              {{ formataReais(imovel.valor_de_venda) }}
            </v-card-title>
          </v-img>

          <v-card-subtitle>
            ... {{ imovel.descricao.slice(-110) }}</v-card-subtitle
          >

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
      return valor.toLocaleString("pt-BR", {
        style: "currency",
        currency: "BRL",
      });
    },
  },
};
</script>