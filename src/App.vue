<template>
  <v-app id="inspire">
    <v-app-bar app color="white" flat>
      <v-avatar :color="$vuetify.breakpoint.smAndDown ? 'grey darken-1' : 'transparent'" size="32"></v-avatar>

      <v-tabs centered class="ml-n9" color="grey darken-1">
        <v-tab v-for="link in links" :key="link">
          {{ link }}
        </v-tab>
      </v-tabs>

      <v-avatar class="hidden-sm-and-down" color="grey darken-1 shrink" size="32"></v-avatar>
    </v-app-bar>

    <v-main class="grey lighten-3">
      <v-container :fluid="true">
        <v-row>
          <v-col cols="12" sm="3">

            <v-expansion-panels
              :accordion="true"
              :flat="false"
              :inset="false"
              :multiple="true"
              :popout="false"
            >
              <!-- Filtro de Estados e Cidades -->
              <v-expansion-panel>
                <v-expansion-panel-header>
                  <template v-slot:default>
                    <v-row>
                      <v-col cols="12">
                        <v-text-field v-model="search" label="Estado ou cidade ..."   
                        hide-details
                        clearable 
                        clear-icon="mdi-close-circle-outline">
                        </v-text-field>
                      </v-col>
                      <v-col cols="12">
                        <v-chip v-for="node in cidades_selecionadas"
                        :key="node.val"
                        small
                        close
                        color="teal"
                        text-color="white"
                          > {{ node.val }} ({{ node.count }}) 
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
                        :items="treeItems.buckets"
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

              <!-- Filtro Valor -->
              <v-expansion-panel>
                <v-expansion-panel-header>
                  Valor de venda
                </v-expansion-panel-header>
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

              <!-- Filtros de Categorias -->
              <v-expansion-panel
                v-for="(filtro, filtro_idx) in filtros "
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
            <v-sheet rounded="lg" class="fill-height pa-0">
              
              <!--  -->
            </v-sheet>
          </v-col>

          <v-col cols="12" sm="9">
            <v-sheet min-height="70vh" rounded="lg">
              <!--  -->
            </v-sheet>
          </v-col>

        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import axios from 'axios';  // Importe o Axios

export default {
  data: () => ({
    links: [
      'Busca Imóveis'
    ],
    treeItems: [
      {
        id: 1,
        name: 'Applications :',
        children: [
          { id: 2, name: 'Calendar : app' },
          { id: 3, name: 'Chrome : app' },
          { id: 4, name: 'Webstorm : app' },
        ],
      },
      {
        id: 5,
        name: 'Documents :',
        children: [
          {
            id: 6,
            name: 'vuetify :',
            children: [
              {
                id: 7,
                name: 'src :',
                children: [
                  { id: 8, name: 'index : ts' },
                  { id: 9, name: 'bootstrap : ts' },
                ],
              },
            ],
          },
          {
            id: 10,
            name: 'material2 :',
            children: [
              {
                id: 11,
                name: 'src :',
                children: [
                  { id: 12, name: 'v-btn : ts' },
                  { id: 13, name: 'v-card : ts' },
                  { id: 14, name: 'v-window : ts' },
                ],
              },
            ],
          },
        ],
      },
      {
        id: 15,
        name: 'Downloads :',
        children: [
          { id: 16, name: 'October : pdf' },
          { id: 17, name: 'November : pdf' },
          { id: 18, name: 'Tutorial : html' },
        ],
      },
      {
        id: 19,
        name: 'Videos :',
        children: [
          {
            id: 20,
            name: 'Tutorials :',
            children: [
              { id: 21, name: 'Basic layouts : mp4' },
              { id: 22, name: 'Advanced techniques : mp4' },
              { id: 23, name: 'All about app : dir' },
            ],
          },
          { id: 24, name: 'Intro : mov' },
          { id: 25, name: 'Conference introduction : avi' },
        ],
      },
    ],
    filtros : {},
    cidades_selecionadas: [],
    search: null,
    caseSensitive: false,
  }),
  computed: {
    filter() {
      return this.caseSensitive
        ? (item, search, textKey) => item[textKey].indexOf(search) > -1
        : undefined
    },
  },
  mounted() {
    // Faça a chamada para o arquivo 'facets.js' e processe o resultado
    axios.get('/api/facets.js')
      .then(response => {
        // Supondo que 'facets.js' tenha uma estrutura de dados similar ao que você forneceu
        // Preencha a propriedade 'treeItems' com os dados da resposta
        console.log(response.data.facets);

        let remove_chaves = ['estados', 'count', 'cidades'];

        for (let filtro in response.data.facets) {
          console.log(filtro);
          console.log(remove_chaves.includes(filtro));
          if (!remove_chaves.includes(filtro)) {
            console.log(filtro, response.data.facets[filtro]);
            this.filtros[filtro] = response.data.facets[filtro];
          }
        }

        this.treeItems = response.data.facets.estados;  // Supondo que os dados estejam em 'facets' na resposta
      })
      .catch(error => {
        // Lide com erros de solicitação aqui
        console.error(error);
      });
  },
  methods: {
    logItem(item) {
      // Esta função será chamada para cada item na árvore
      // e exibirá o item no console.log
      console.log(item);

      // Você pode retornar qualquer string desejada para ser exibida como texto do item
      return item.val; // Neste exemplo, estamos retornando o valor "val" do item
    },
    // ...
  },
}
</script>