<template>
  <v-expansion-panel>
    <v-expansion-panel-header> Valor de venda </v-expansion-panel-header>
    <v-expansion-panel-content>
      <!-- um slider com campos de texto para o valor mínimo e máximo dos imóveis -->
      <v-row>
        <v-col cols="12">
          <table>
            <tr>
              <th>Min</th>
              <th>25%</th>
              <th>50%</th>
              <th>75%</th>
              <th>95%</th>
              <th>Max</th>
            </tr>
            <tr>
              <td>{{ formatarNumeroLegivel(valor_menor.min) }}</td>
              <td>{{ formatarNumeroLegivel(valor_menor.percentiles[1]) }}</td>
              <td>{{ formatarNumeroLegivel(valor_menor.percentiles[3]) }}</td>
              <td>{{ formatarNumeroLegivel(valor_menor.percentiles[5]) }}</td>
              <td>{{ formatarNumeroLegivel(valor_menor.percentiles[7]) }}</td>
              <td>{{ formatarNumeroLegivel(valor_menor.max) }}</td>
            </tr>
          </table>
        </v-col>
      </v-row>
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

<style>
  table {
    border-collapse: collapse;
    width: 100%;
  }

  th, td {
    border: 1px solid #dddddd;
    text-align: center;
    padding: 8px;
  }

  th {
    background-color: #f2f2f2;
  }
</style>

<script>
import Plotly from "plotly.js-dist-min";

export default {
  name: "FiltroValor",
  props: ["valor_menor"],
  mounted: function () {
    var trace1 = {
      x: [1, 2, 3, 4, 4, 4, 8, 9, 10],
      type: "box",
      name: "Set 1",
    };

    var trace2 = {
      x: [2, 3, 3, 3, 3, 5, 6, 6, 7],
      type: "box",
      name: "Set 2",
    };

    var data = [trace1, trace2];

    var layout = {
      title: "Horizontal Box Plot",
    };

    if (document.getElementById("myDiv") != null) {
      Plotly.newPlot("myDiv", data, layout);
    }

    console.log(Plotly);
  },
  methods: {
    formatarNumeroLegivel(numero) {
      if (numero >= 1000000) {
        return (numero / 1000000).toFixed(1) + " mi";
      } else if (numero >= 1000) {
        return (numero / 1000).toFixed(1) + " mil";
      } else {
        return numero.toFixed(1);
      }
    }
  }
};
</script>
