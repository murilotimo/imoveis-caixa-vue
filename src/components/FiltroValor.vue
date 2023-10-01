<template>
  <v-expansion-panel>
    <v-expansion-panel-header> Valor de venda </v-expansion-panel-header>
    <v-expansion-panel-content>
      <!-- um slider com campos de texto para o valor mínimo e máximo dos imóveis -->
      <v-row>
        <v-col cols="12" sm="6">
          <v-text-field
            label="Valor mínimo"
            prepend-icon="mdi-cash"
            type="number"
            min="0"
            step="1000"
            outlined
          ></v-text-field>
        </v-col>
        <v-col cols="12" sm="6">
          <v-text-field
            label="Valor máximo"
            prepend-icon="mdi-cash"
            type="number"
            min="0"
            step="1000"
            outlined
          ></v-text-field>
        </v-col>
      </v-row>
    </v-expansion-panel-content>
  </v-expansion-panel>
</template>

<script>
export default {
  name: "FiltroValor",
  props: ["Valor"],
};
</script>
