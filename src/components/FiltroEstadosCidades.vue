<template>
  <v-expansion-panel>
    <v-expansion-panel-header>
      <template v-slot:default>
        <v-row>
          <v-col cols="12">
            <v-text-field
              v-model="search"
              label="Estado ou cidade ..."
              hide-details
              clearable
              clear-icon="mdi-close-circle-outline"
            >
            </v-text-field>
          </v-col>
          <v-col cols="12">
            <v-chip
              v-for="node in cidades_selecionadas"
              :key="node.val"
              small
              close
              color="teal"
              text-color="white"
            >
              {{ node.val }} ({{ node.count }})
            </v-chip>
          </v-col>
        </v-row>
      </template>
    </v-expansion-panel-header>
    <v-expansion-panel-content>
      <v-card-text>
        <v-treeview
          selectable
          hoverable
          open-on-click
          dense
          return-object
          v-model="cidades_selecionadas"
          :items="estados.buckets"
          item-children="cidades.buckets"
          item-text="val"
          item-key="val"
          :search="search"
          :filter="filter"
        >
          <template v-slot:append="{ item }">
            {{ item.count }}
          </template>
        </v-treeview>
      </v-card-text>
    </v-expansion-panel-content>
  </v-expansion-panel>
</template>

<script>
export default {
  name: "FilterEstadosCidades",
  props: ["estados", "selecionadas"],
  data: () => ({
    cidades_selecionadas: [],
    search: null,
  }),
  computed: {
    filter() {
      return this.caseSensitive
        ? (item, search, textKey) => item[textKey].indexOf(search) > -1
        : undefined;
    },
  },
  watch: {
    cidades_selecionadas: function (val) {
        console.log("update_cidades_selecionadas", val);
        this.$emit("update_cidades_selecionadas", val);
    },
  },
  created() {
    if (
      this.selecionadas !== null &&
      typeof this.selecionadas !== "undefined"
    ) {
      this.cidades_selecionadas = this.selecionadas;
    }
  },
};
</script>
