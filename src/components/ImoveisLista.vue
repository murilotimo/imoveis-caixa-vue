<template>
  <v-container fluid>
    <v-card color="grey lighten-4" flat height="10px" tile> </v-card>
    <v-row>
      <v-col
        v-for="imovel in imoveis"
        :key="imovel.id_imovel"
        cols="12"
        xl="3"
        lg="4"
        sm="12"
        xs="12"
        class="pa-0"
      >
        <v-card class="imovel-card" outlined tile>
          <v-carousel height="230">
            <v-carousel-item
              v-for="(item, i) in imovel['images.path']"
              :key="i"
            >
              <img
                :src="
                  'https://construcaocompartilhada.com.br/leiloes/imagens_imoveis/' +
                  item
                "
                alt="Imagem do Imóvel"
              />
            </v-carousel-item>
          </v-carousel>

          <v-card-subtitle v-if="imovel.dh_fim_venda_online">{{
            formatarDataHora(imovel.dh_fim_venda_online)
          }}</v-card-subtitle>

          <v-card-subtitle>{{ imovel.descricao }}</v-card-subtitle>

          <v-card-text>
            <v-row dense>
              <v-col cols="6" class="d-flex" v-if="imovel.desconto">
                <v-row dense>
                  <v-col cols="12">
                    <v-chip class="ma-0" x-small color="green darken-2"
                      >Desconto</v-chip
                    >
                  </v-col>
                  <v-col cols="12">
                    <v-card-title>{{
                      formatarParaPorcentagem(imovel.desconto)
                    }}</v-card-title>
                  </v-col>
                </v-row>
              </v-col>
              <v-col cols="6" class="d-flex">
                <v-row dense>
                  <v-chip class="ma-2" x-small color="blue darken-2"
                    >Avaliação</v-chip
                  >
                  <v-card-subtitle>{{
                    formataReais(imovel.valor_de_avaliacao)
                  }}</v-card-subtitle>
                </v-row>
              </v-col>
              <v-col col="12" class="d-flex" v-if="imovel.valor_de_venda">
                <v-chip class="ma-2" x-small color="blue darken-2"
                  >Venda</v-chip
                >
                <v-card-title>{{
                  formataReais(imovel.valor_de_venda)
                }}</v-card-title>
              </v-col>
              <v-col
                col="12"
                class="d-flex"
                v-if="imovel.valor_minimo_de_venda"
              >
                <v-chip class="ma-2" x-small color="green darken-2"
                  >Mínimo</v-chip
                >
                <v-card-title>{{
                  formataReais(imovel.valor_minimo_de_venda)
                }}</v-card-title>
              </v-col>
              <v-col
                col="12"
                class="d-flex"
                v-if="imovel.valor_minimo_de_venda_a_vista"
              >
                <v-chip class="ma-2" x-small color="amber darken-2"
                  >À vista</v-chip
                >
                <v-card-title>{{
                  formataReais(imovel.valor_minimo_de_venda_a_vista)
                }}</v-card-title>
              </v-col>
            </v-row>
            <v-row dense>
              <v-col v-if="imovel.area_privativa">
                <span>Área Privativa</span>
                <v-card-subtitle
                  >{{ imovel.area_privativa }}
                  {{ imovel.area_privativa_unidade }}</v-card-subtitle
                >
              </v-col>
              <v-col v-if="imovel.area_total">
                <span>Área Total</span>
                <v-card-subtitle
                  >{{ imovel.area_total }}
                  {{ imovel.area_total_unidade }}</v-card-subtitle
                >
              </v-col>
              <v-col v-if="imovel.terreno">
                <span>Terreno</span>
                <v-card-subtitle
                  >{{ imovel.terreno }}
                  {{ imovel.terreno_unidade }}</v-card-subtitle
                >
              </v-col>
            </v-row>

            <v-row dense>
              <v-col v-if="imovel.tipo_de_imovel">
                <span>Tipo</span>
                <v-card-subtitle>{{ imovel.tipo_de_imovel }}</v-card-subtitle>
              </v-col>
              <v-col v-if="imovel.quartos">
                <span>Quartos</span>
                <v-card-subtitle>{{ imovel.quartos }}</v-card-subtitle>
              </v-col>
              <v-col v-if="imovel.garagem">
                <span>Garagem</span>
                <v-card-subtitle>{{ imovel.garagem }}</v-card-subtitle>
              </v-col>
            </v-row>

            <v-card-subtitle>{{ imovel.endereco }}</v-card-subtitle>
          </v-card-text>

          <v-card-actions>
            <v-btn
              class="v-btn v-btn--block white--text"
              color="orange"
              :href="`https://venda-imoveis.caixa.gov.br/sistema/detalhe-imovel.asp?hdnOrigem=index&hdnimovel=${imovel.id_imovel}`"
              target="_blank"
            >
              Visitar
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<style scoped>
/* Estilos personalizados */
.imovel-card {
  margin: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

/* Você pode personalizar as cores das fichas conforme desejado */
.blue.darken-2 {
  background-color: #1976d2;
  color: white;
}

.green.darken-2 {
  background-color: #43a047;
  color: white;
}

.amber.darken-2 {
  background-color: #ffb300;
  color: white;
}
</style>

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
    formatarParaPorcentagem(valor) {
      // Verifica se o valor é um número
      if (typeof valor !== "number") {
        return "Valor inválido";
      }

      // Converte o valor em uma string com duas casas decimais
      const valorFormatado = (Math.abs(valor) * 100).toFixed(2);

      // Adiciona o sinal de porcentagem
      return valorFormatado + " %";
    },
    formatarDataHora(string) {
      return new Date(string).toLocaleString("pt-BR", { timeZone: "UTC" });
    },
  },
};
</script>
