<template>
  <v-app id="inspire">
    <v-app-bar app color="white" flat>
      <v-avatar
        :color="$vuetify.breakpoint.smAndDown ? 'grey darken-1' : 'transparent'"
        size="32"
      ></v-avatar>

      <v-tabs centered class="ml-n9" color="grey darken-1">
        <v-tab v-for="link in links" :key="link">
          {{ link }}
        </v-tab>
      </v-tabs>

      <v-avatar
        class="hidden-sm-and-down"
        color="grey darken-1 shrink"
        size="32"
      ></v-avatar>
    </v-app-bar>

    <v-main class="grey lighten-3">
      <v-container :fluid="true">
        <v-row>
          <v-col cols="12" sm="3">
            <filtros-component
              :filtros="filtros"
              :filtrosSelecionados="filtrosSelecionados"
              @update_filtros_categoria="update_filtros_categoria"
            />
            <v-sheet rounded="lg" class="fill-height pa-0">
              <!--  -->
            </v-sheet>
          </v-col>

          <v-col cols="12" sm="9">
            <v-sheet min-height="70vh" rounded="lg">
              <v-toolbar>
                <v-toolbar-title>
                  <p>
                    Foram encontrados
                    {{ pTotalImoveis.toLocaleString("pt-BR") }}
                  </p>
                </v-toolbar-title>

                <v-spacer></v-spacer>

                <v-col class="d-flex" cols="3">
                  <v-select
                    :items="sortOptions"
                    v-model="sort"
                    item-text="nome"
                    item-value="valor"
                    label="Ordenar por"
                    outlined
                    dense
                  >
                    <v-option> </v-option>
                  </v-select>
                </v-col>
              </v-toolbar>
              <!--  -->
              <ImoveisLista :imoveis="imoveis"></ImoveisLista>
              <!--Add here the vuetify directive -->
              <v-card v-intersect="infiniteScrolling"></v-card>
            </v-sheet>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import axios from "axios"; // Importe o Axios
import FiltrosComponent from "./components/FiltrosComponent.vue";
import ImoveisLista from "./components/ImoveisLista.vue";
import _ from "lodash";

export default {
  components: {
    FiltrosComponent,
    ImoveisLista,
  },
  data: () => ({
    links: ["Busca Imóveis"],
    filtros: {
      tipo_de_imovel: {
        missing: { count: 0 },
        buckets: [],
      },
      quartos: {
        missing: { count: 0 },
        buckets: [],
      },
      garagem: {
        missing: { count: 0 },
        buckets: [],
      },
      averbacao_dos_leiloes_negativos: {
        missing: { count: 0 },
        buckets: [],
      },
      condicoes: { missing: { count: 1 }, buckets: [] },
      estados: { missing: { count: 0 }, buckets: [] },
    },
    imoveis: [
      {
        uf: "DF",
        cidade: "BRASILIA",
        id_imovel: "01555527580486",
        valor_de_avaliacao: 210000,
        valor_de_venda: 210000,
        tipo_de_imovel: "Apartamento",
        quartos: 2,
        garagem: 1,
        numero_do_imovel: "155552758048-6",
        matricula: "294102",
        comarca: "BRASILIA-DF",
        oficio: "03",
        inscricao_imobiliaria: "Não identificado",
        averbacao_dos_leiloes_negativos: "Não se aplica",
        area_privativa: [44.66],
        area_privativa_unidade: "m2",
        endereco:
          "QD CSG 11,N. SN APTO. 905 LTS 1 E 2, TAGUATINGA SUL (TAGUATINGA) - CEP: 72035-511, BRASILIA - DISTRITO FEDERAL",
        descricao:
          "2  Quartos, 1 Vaga na Garagem,  Área de Serviço,  Wc,  Sala,  Cozinha. Penhora Gravada no R-7 da Matrícula - Regularização Por Conta do Adquirente. Vaga de Garagem nº 96.",
        condicoes: [
          "Imóvel NÃO aceita utilização de FGTS.",
          "FINANCIAMENTO (consulte condições).",
          "Imóvel NÃO aceita parcelamento.",
          "Imóvel NÃO aceita consórcio.",
        ],
        baixar_edital_e_anexos: "/editais/EL00080223CPARE.PDF",
        baixar_matricula_do_imovel: "/editais/matricula/DF/1555527580486.pdf",
        edital: "Leilão SFI - Edital Único 0008/0223 - CPA/RE",
        numero_do_item: "15",
        edital_publicado_em: "2023-09-20T16:27:33Z",
        id: "01555527580486",
        _version_: 1778518024205107200,
      },
      {
        uf: "DF",
        cidade: "BRASILIA",
        id_imovel: "01555523260177",
        valor_de_avaliacao: 315000,
        valor_de_venda: 315000,
        tipo_de_imovel: "Apartamento",
        quartos: 2,
        garagem: 1,
        numero_do_imovel: "155552326017-7",
        matricula: "313089",
        comarca: "BRASILIA-DF",
        oficio: "03",
        inscricao_imobiliaria: "52610764",
        averbacao_dos_leiloes_negativos: "Não se aplica",
        area_privativa: [52.26],
        area_privativa_unidade: "m2",
        endereco:
          "QUADRA QI 24  APTO. 1807 TR D, LT 14 A 27, SETOR INDUSTRIAL (TAGUATINGA) - CEP: 72135-240, BRASILIA - DISTRITO FEDERAL",
        descricao:
          "2  Quartos, 1 Vaga na Garagem,  Área de Serviço,  Wc,  Sala,  Cozinha. Vaga de Garagem nº 283.",
        condicoes: [
          "Imóvel NÃO aceita utilização de FGTS.",
          "FINANCIAMENTO (consulte condições).",
          "Imóvel NÃO aceita parcelamento.",
          "Imóvel NÃO aceita consórcio.",
          "Imóvel com ação judicial: 00163829220174013400.",
        ],
        baixar_edital_e_anexos: "/editais/EL00120223CPARE.PDF",
        baixar_matricula_do_imovel: "/editais/matricula/DF/1555523260177.pdf",
        edital: "Leilão SFI - Edital Único 0012/0223 - CPA/RE",
        numero_do_item: "31",
        edital_publicado_em: "2023-09-20T12:07:55Z",
        id: "01555523260177",
        _version_: 1778518287723790300,
      },
      {
        uf: "DF",
        cidade: "BRASILIA",
        id_imovel: "01787700096413",
        valor_de_avaliacao: 284000,
        valor_de_venda: 296895.03,
        tipo_de_imovel: "Apartamento",
        quartos: 3,
        garagem: 1,
        numero_do_imovel: "178770009641-3",
        matricula: "315519",
        comarca: "TAGUATINGA-DF",
        oficio: "03",
        inscricao_imobiliaria: "52627357",
        averbacao_dos_leiloes_negativos: "Não se aplica",
        area_privativa: [50.52],
        area_privativa_unidade: "m2",
        endereco:
          "Q QI 24,N. S/N APTO. 610 LOTE 1 A 13 TORRE F, SETOR INDUSTRIAL (TAGUATINGA) - CEP: 72135-240, BRASILIA - DISTRITO FEDERAL",
        descricao:
          "3  Quartos, 1 Vaga na Garagem,  Área de Serviço, 2 Wc,  Sala,  Cozinha. Equipamento de Segurança, Espaço Com Churrasqueira, Rede Interna de Gás, Piscina, Playground, Portaria E/ou Guarita, Salão de Festas, Outros Salões Delazer, Sauna / Ofurô / Hidromassagem, Quadra de Tênis, Quadra Poliesportiva.\r\n.",
        condicoes: [
          "FINANCIAMENTO (consulte condições).",
          "Imóvel NÃO aceita parcelamento.",
          "Imóvel NÃO aceita consórcio.",
        ],
        baixar_edital_e_anexos: "/editais/ES31420223CPARE.PDF",
        baixar_matricula_do_imovel: "/editais/matricula/DF/1787700096413.pdf",
        edital: "2º Leilão SFI 3142/0223 - CPA/RE",
        numero_do_item: "19",
        edital_publicado_em: "2023-09-20T15:30:04Z",
        id: "01787700096413",
        _version_: 1778518289937334300,
      },
      {
        uf: "DF",
        cidade: "BRASILIA",
        id_imovel: "08444423188632",
        valor_de_avaliacao: 205000,
        valor_de_venda: 205000,
        tipo_de_imovel: "Apartamento",
        quartos: 2,
        garagem: 1,
        numero_do_imovel: "844442318863-2",
        matricula: "355755",
        comarca: "BRASILIA-DF",
        oficio: "03",
        inscricao_imobiliaria: "5312250X",
        averbacao_dos_leiloes_negativos: "Não se aplica",
        area_privativa: [52.01],
        area_privativa_unidade: "m2",
        endereco:
          "QS 517,N. SN APTO. 306 LT 01, CJ A, SAMAMBAIA SUL (SAMAMBAIA) - CEP: 72315-551, BRASILIA - DISTRITO FEDERAL",
        descricao:
          "2  Quartos, 1 Vaga na Garagem,  Área de Serviço,  Wc,  Sala,  Cozinha. Vaga de Garagem n° 72.",
        condicoes: [
          "FINANCIAMENTO (consulte condições).",
          "Imóvel NÃO aceita parcelamento.",
          "Imóvel NÃO aceita consórcio.",
        ],
        baixar_edital_e_anexos: "/editais/EL00080223CPARE.PDF",
        baixar_matricula_do_imovel: "/editais/matricula/DF/8444423188632.pdf",
        edital: "Leilão SFI - Edital Único 0008/0223 - CPA/RE",
        numero_do_item: "13",
        edital_publicado_em: "2023-09-20T16:27:33Z",
        id: "08444423188632",
        _version_: 1778518295276683300,
      },
      {
        uf: "DF",
        cidade: "BRASILIA",
        id_imovel: "00000240019016",
        valor_de_avaliacao: 155000,
        valor_de_venda: 91450,
        tipo_de_imovel: "Apartamento",
        quartos: 2,
        garagem: 1,
        numero_do_imovel: "000024001901-6",
        matricula: "233470",
        comarca: "BRASILIA-DF",
        oficio: "03",
        inscricao_imobiliaria: "50060996",
        averbacao_dos_leiloes_negativos: "Não se aplica",
        area_total: 47.52,
        area_total_unidade: "m2",
        area_privativa: [43.15],
        area_privativa_unidade: "m2",
        endereco:
          "QN 108 CONJ 01 LOTE 01  APTO. 102 BL B ENTRADA C, SAMAMBAIA SUL (SAMAMBAIA) - CEP: 70302-251, BRASILIA - DISTRITO FEDERAL",
        descricao:
          "2  Quartos, 1 Vaga na Garagem,  Área de Serviço,  Wc,  Sala,  Cozinha. Imóvel Far 440154265197. Matrícula Caixa n. 233.470 - 3. ri de Brasilia/df. Sem Utilização de Fgts. Com Ação Jud. Proc. Nr. 00157389120134013400 - 5 vf de Brasilia/df.\r\n.",
        condicoes: [
          "Imóvel NÃO aceita utilização de FGTS.",
          "Imóvel NÃO aceita financiamento habitacional.",
          "Imóvel NÃO aceita parcelamento.",
          "Imóvel NÃO aceita consórcio.",
        ],
        baixar_edital_e_anexos: "/editais/EC00062023FARVESP.PDF",
        baixar_matricula_do_imovel: "/editais/matricula/DF/0000240019016.pdf",
        edital: "Concorrência Pública 0006/2023 - FARVE/SP",
        numero_do_item: "76",
        edital_publicado_em: "2023-09-21T15:53:58Z",
        id: "00000240019016",
        _version_: 1778518297400049700,
      },
      {
        uf: "DF",
        cidade: "BRASILIA",
        id_imovel: "00000240019059",
        valor_de_avaliacao: 141000,
        valor_de_venda: 83190,
        tipo_de_imovel: "Apartamento",
        quartos: 2,
        garagem: 1,
        numero_do_imovel: "000024001905-9",
        matricula: "250245",
        comarca: "BRASILIA-DF",
        oficio: "03",
        inscricao_imobiliaria: "Não identificado",
        averbacao_dos_leiloes_negativos: "Não se aplica",
        area_total: 48.77,
        area_total_unidade: "m2",
        area_privativa: [43.39],
        area_privativa_unidade: "m2",
        endereco:
          "QN 104 CONJ 01 LOTE 1,N. SN APTO. 102 BL 06, SAMAMBAIA SUL (SAMAMBAIA) - CEP: 72302-051, BRASILIA - DISTRITO FEDERAL",
        descricao:
          "2  Quartos, 1 Vaga na Garagem,  Área de Serviço,  Wc,  Sala,  Cozinha. Imóvel Far 440175406082. Matrícula Caixa nº 250245 - 3º Ori de Brasilia/df.  Com Ação Jud. Proc. Nr. 10401338620204013400 - 24ª Jef de Brasilia/df e 10255921920184013400 - 6ª vf de Brasilia/df. Sem Utilização de Fgts.\r\n.",
        condicoes: [
          "Imóvel NÃO aceita utilização de FGTS.",
          "Imóvel NÃO aceita financiamento habitacional.",
          "Imóvel NÃO aceita parcelamento.",
          "Imóvel NÃO aceita consórcio.",
        ],
        baixar_edital_e_anexos: "/editais/EC00062023FARVESP.PDF",
        baixar_matricula_do_imovel: "/editais/matricula/DF/0000240019059.pdf",
        edital: "Concorrência Pública 0006/2023 - FARVE/SP",
        numero_do_item: "75",
        edital_publicado_em: "2023-09-21T15:53:58Z",
        id: "00000240019059",
        _version_: 1778518298462257200,
      },
      {
        uf: "DF",
        cidade: "BRASILIA",
        id_imovel: "01555519569444",
        valor_de_avaliacao: 192200,
        valor_de_venda: 192200,
        tipo_de_imovel: "Apartamento",
        quartos: 2,
        garagem: 1,
        numero_do_imovel: "155551956944-4",
        matricula: "312027",
        comarca: "TAGUATINGA-DF",
        oficio: "03",
        inscricao_imobiliaria: "51989883",
        averbacao_dos_leiloes_negativos: "Não se aplica",
        area_privativa: [44.96],
        area_privativa_unidade: "m2",
        endereco:
          "QD QS 103  APTO. 602 LT 02 CJ 02, SAMAMBAIA SUL (SAMAMBAIA) - CEP: 72301-500, BRASILIA - DISTRITO FEDERAL",
        descricao:
          "2  Quartos, 1 Vaga na Garagem,  Wc,  Sala,  Cozinha. Vaga de Garagem nº 08.",
        condicoes: [
          "FINANCIAMENTO (consulte condições).",
          "Imóvel NÃO aceita parcelamento.",
          "Imóvel NÃO aceita consórcio.",
        ],
        baixar_edital_e_anexos: "/editais/EL00120223CPARE.PDF",
        baixar_matricula_do_imovel: "/editais/matricula/DF/1555519569444.pdf",
        edital: "Leilão SFI - Edital Único 0012/0223 - CPA/RE",
        numero_do_item: "30",
        edital_publicado_em: "2023-09-20T12:07:55Z",
        id: "01555519569444",
        _version_: 1778518299522367500,
      },
      {
        uf: "DF",
        cidade: "BRASILIA",
        id_imovel: "01555523773646",
        valor_de_avaliacao: 490000,
        valor_de_venda: 490000,
        tipo_de_imovel: "Apartamento",
        quartos: 2,
        garagem: 1,
        numero_do_imovel: "155552377364-6",
        matricula: "269970",
        comarca: "BRASILIA-DF",
        oficio: "03",
        inscricao_imobiliaria: "51563401",
        averbacao_dos_leiloes_negativos: "Não se aplica",
        area_privativa: [69.3],
        area_privativa_unidade: "m2",
        endereco:
          "RUA 13 NORTE LT 1/3,N. S/N APTO. 103 BLOCO D, RUA 14 NORTE LT 2/4, NORTE (AGUAS CLARAS) - CEP: 71000-000, BRASILIA - DISTRITO FEDERAL",
        descricao:
          "2  Quartos, 1 Vaga na Garagem,  Wc,  Sala,  Cozinha. Vaga de Garagem nº 939. Gravame Averbado na Matricula (r-10, R-11, R-12 e Av.13). Regularização Por Conta do Adquirente.",
        condicoes: [
          "Imóvel NÃO aceita utilização de FGTS.",
          "FINANCIAMENTO (consulte condições).",
          "Imóvel NÃO aceita parcelamento.",
          "Imóvel NÃO aceita consórcio.",
          "Imóvel com ação judicial: 07185910420178070001, 00228979120168070001, 00228960920168070001.",
        ],
        baixar_edital_e_anexos: "/editais/EL00090223CPARE.PDF",
        baixar_matricula_do_imovel: "/editais/matricula/DF/1555523773646.pdf",
        edital: "Leilão SFI - Edital Único 0009/0223 - CPA/RE",
        numero_do_item: "14",
        edital_publicado_em: "2023-09-19T07:59:36Z",
        id: "01555523773646",
        _version_: 1778518325129642000,
      },
      {
        uf: "DF",
        cidade: "BRASILIA",
        id_imovel: "08555511342376",
        valor_de_avaliacao: 594000,
        valor_de_venda: 594000,
        tipo_de_imovel: "Apartamento",
        quartos: 3,
        garagem: 2,
        numero_do_imovel: "855551134237-6",
        matricula: "249209",
        comarca: "BRASILIA-DF",
        oficio: "03",
        inscricao_imobiliaria: "51090716",
        averbacao_dos_leiloes_negativos: "Não se aplica",
        area_privativa: [96.9],
        area_privativa_unidade: "m2",
        endereco:
          "QUADRA 209  APTO. 502 BL A, AGUAS CLARAS - CEP: 71930-750, BRASILIA - DISTRITO FEDERAL",
        descricao:
          "3  Quartos, 2  Vagas na Garagem,  Varanda/sacada,  Área de Serviço, 2 Wc,  Sala,  Cozinha. Vagas de Garagem nº 47 e nº 47-a. o Condomínio Possui Espaço Com Churrasqueira, Piscina, Portaria E/ou Guarita, Salão de Festas.",
        condicoes: [
          "Imóvel NÃO aceita utilização de FGTS.",
          "FINANCIAMENTO (consulte condições).",
          "Imóvel NÃO aceita parcelamento.",
          "Imóvel NÃO aceita consórcio.",
          "Imóvel com ação judicial: 07229080920218070000, 07149022120198070020.",
        ],
        baixar_edital_e_anexos: "/editais/EL00130223CPARE.PDF",
        baixar_matricula_do_imovel: "/editais/matricula/DF/8555511342376.pdf",
        edital: "Leilão SFI - Edital Único 0013/0223 - CPA/RE",
        numero_do_item: "26",
        edital_publicado_em: "2023-09-21T09:53:11Z",
        id: "08555511342376",
        _version_: 1778518334768152600,
      },
    ],
    filtrosSelecionados: {
      filtrosCategorias: {
        cidade: [],
      },
      filtrosValores: {},
    },
    sortOptions: [
      {
        nome: "Maior Desconto",
        valor:
          "sub(div(min(valor_de_avaliacao,valor_de_venda,valor_minimo_de_venda,valor_minimo_de_venda_a_vista), valor_de_avaliacao),1) asc",
      },
      {
        nome: "Menor Valor",
        valor:
          "min(valor_de_avaliacao,valor_de_venda,valor_minimo_de_venda,valor_minimo_de_venda_a_vista) asc",
      },
      {
        nome: "Maior Área",
        valor: "max(area_do_terreno, area_privativa, area_total) desc",
      },
      { nome: "Menor Valor de Venda", valor: "valor_de_venda asc" },
      { nome: "Maior Valor de Venda", valor: "valor_de_venda desc" },
      { nome: "Menor Valor de Avaliação", valor: "valor_de_avaliacao asc" },
      { nome: "Maior Valor de Avaliação", valor: "valor_de_avaliacao desc" },
    ],
    sort: "",
    search: null,
    caseSensitive: false,
    pTotalImoveis: 0,
    pPaginaAtual: 0,
    pTamanhoPagina: 40,
  }),
  computed: {
    filter() {
      return this.caseSensitive
        ? (item, search, textKey) => item[textKey].indexOf(search) > -1
        : undefined;
    },
  },
  beforeCreate() {
    // Faça a chamada para o arquivo 'facets.js' e processe o resultado
    axios
      .get("http://localhost:5002/imoveis/")
      .then((response) => {
        // Supondo que 'facets.js' tenha uma estrutura de dados similar ao que você forneceu
        // Preencha a propriedade 'treeItems' com os dados da resposta
        console.log(response.data);

        // Ajusta quantida de imóveis encontratos
        this.pTotalImoveis = response.data.response.numFound;

        let remove_chaves = ["estados", "count", "cidades"];

        for (let filtro in response.data.facets) {
          //console.log(filtro);
          //console.log(remove_chaves.includes(filtro));
          if (!remove_chaves.includes(filtro)) {
            //console.log(filtro, response.data.facets[filtro]);
            this.filtros[filtro] = response.data.facets[filtro];
          }
        }

        this.filtros["estados"] = response.data.facets.estados; // Supondo que os dados estejam em 'facets' na resposta
      })
      .catch((error) => {
        // Lide com erros de solicitação aqui
        console.error(error);
      });
  },
  created: function () {
    console.log("Parametros da Url", this.$route.query);

    // Recupere a string codificada dos parâmetros de consulta da URL
    const encodedJsonString = this.$route.query.filtros;

    // Decodifique a string para obter o objeto JavaScript original
    console.log(encodedJsonString);
    if (encodedJsonString) {
      const jsonString = decodeURIComponent(encodedJsonString);
      const queryParams = JSON.parse(jsonString);
      this.filtrosSelecionados = queryParams;
    }

    // Agora, queryParams conterá o objeto original com os filtros
    //console.log(queryParams.filtrosCategoria.cidade); // Irá mostrar a lista de cidades

    // Use o Vue Router para obter os parâmetros da URL
    //this.filtrosSelecionados = Object.assign({}, this.$route.query);

    /*
    // Use o Vue Router para obter os parâmetros da URL
    this.pPaginaAtual = this.$route.query.pagina
      ? parseInt(this.$route.query.pagina)
      : 0;

    // Use o Vue Router para obter os parâmetros da URL
    this.pTamanhoPagina = this.$route.query.tamanhoPagina
      ? parseInt(this.$route.query.tamanhoPagina)
      : 25;
    */

    this.getImoveis();
  },
  methods: {
    logItem(item) {
      // Esta função será chamada para cada item na árvore
      // e exibirá o item no console.log
      console.log(item);

      // Você pode retornar qualquer string desejada para ser exibida como texto do item
      return item.val; // Neste exemplo, estamos retornando o valor "val" do item
    },
    //captura a informação das cidades selecionadas do componente filtros
    update_filtros_categoria: function (params) {
      console.log("Filtros foram atualizados", params, this);

      //Verifica o tamanho do objeto params
      if (Object.keys(params).length === 0) {
        // Caso esteja vazio remove o parametro da url
        this.filtrosSelecionados["filtrosCategorias"] = {};
        this.$router.push({ path: "/" });
      } else {
        // Caso não esteja vazio, Aplica o filtro e atualiza a url

        for (var [key, value] of Object.entries(params)) {
          console.log("Aquiqqqqqq", key, value);
          var tpFiltro = value["tipoFiltro"];
          var nmFiltro = value["nomeFiltro"];
          var vlFiltro = value["valoresSelecionados"];

          this.filtrosSelecionados[tpFiltro][nmFiltro] = vlFiltro;
        }

        var jsonString = JSON.stringify(this.filtrosSelecionados);
        var encodedJsonString = encodeURIComponent(jsonString);
        var JsonStringAtual = this.$route.query.filtros;

        // Check if the new filters are different from the current filters in the URL
        if (JsonStringAtual != encodedJsonString) {
          this.$router.push({
            path: "/",
            query: { filtros: encodedJsonString },
          });
        }
      }

      this.getImoveis();
    },
    debounced_update_filtros: function (params) {
      _.debouce(this.update_filtros(params), 3000);
    },
    formataReais(numero) {
      const numeroFormatado = numero.toLocaleString("pt-BR", {
        style: "currency",
        currency: "BRL",
        minimumFractionDigits: 2,
      });
      return numeroFormatado;
    },
    formatarInteiro(numero) {
      return numero.toLocaleString("pt-BR");
    },
    getImoveis: function () {
      const self = this;

      let envelope = {
        filtros: self.filtrosSelecionados,
        offset: this.pPaginaAtual * this.pTamanhoPagina,
        limit: this.pTamanhoPagina,
        sort: this.sort,
      };

      console.log("Envelope", envelope);

      axios
        .post(
          "http://localhost:5002/imoveis/busca",
          envelope
          /*
        {
          filtros: self.filtrosSelecionados,
          start: this.pPaginaAtual * this.pTamanhoPagina,
          rows: this.pTamanhoPagina,
        }
        */
        )
        .then((response) => {
          console.log(response.data);
          this.imoveis = response.data.response.docs;
          this.pTotalImoveis = response.data.response.numFound;
        })
        .catch((error) => {
          // Lide com erros de solicitação aqui
          console.error(error);
        });
    },
    infiniteScrolling(entries, observer, isIntersecting) {
      const self = this;
      console.log("Chamou o Scrolllllllllll");
      console.log(entries, observer, isIntersecting);
      this.pPaginaAtual = this.pPaginaAtual + 1;

      let envelope = {
        filtros: self.filtrosSelecionados,
        offset: this.pPaginaAtual * this.pTamanhoPagina,
        limit: this.pTamanhoPagina,
        sort: this.sort,
      };

      axios
        .post(
          "http://localhost:5002/imoveis/busca",
          envelope
        )
        .then((response) => {
          console.log(response.data);
          console.log(response.data.response.docs);
          // Concatena os imoveis atuais com os imoveis recebidos via api
          self.imoveis = self.imoveis.concat(response.data.response.docs);

        })
        .catch((error) => {
          // Lide com erros de solicitação aqui
          console.error(error);
        });
    },
    // ...
  },
  watch: {
    sort: function (val) {
      console.log("Sort", val);
      this.getImoveis();
    },
  },
};
</script>
