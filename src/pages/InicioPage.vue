<template>
  <q-page padding>
    <div class="q-pa-md q-gutter-sm">
      <div class="col-12">
        <q-select
          outlined
          v-model="cod_prg"
          :options="ds_programas"
          stack-label
          option-label="programa"
          option-value="cod_programa"
          label="Programas"
          style="min-width: 800px; max-width: 1200px"
          @update:model-value="get_productos()"
        />
      </div>

      <div class="col-12">
        <q-select
          outlined
          v-model="cod_prd"
          :options="ds_productos"
          option-label="producto"
          option-value="cod_producto"
          label="Productos"
          style="min-width: 800px; max-width: 1200px"
          @update:model-value="get_sub_productos()"
        />
      </div>

      <div class="col-12">
        <q-select
          outlined
          v-model="cod_sub"
          :options="ds_subpro"
          option-label="sub_finalidad"
          option-value="cod_subfinalidad"
          label="Sub Productos"
          style="min-width: 800px; max-width: 1200px"
          @update:model-value="get_metas_subpro()"
        />
      </div>
    </div>

    <div class="q-pa-md q-gutter-sm shadow-5">
      <div class="col-6">
        <q-table
          title="Metas Fisicas 2023"
          :rows="ds_metas"
          :columns="ds_titulo"
          row-key="name"
          no-data-label="sin datos que mostrar"
          :separator="sepa"
          :filter="filter"
          bordered
          square
          title-class="text-bold text-blue-9"
          table-class="bg-grey-2 text-blue-10"
          table-style="border: 1px solid text-blue-10;"
          table-header-style="height: 65px;"
          table-header-class="bg-blue-9 text-white"
          :pagination="initialPagination"
        >
          <template v-slot:top-left>
            <q-input
              outlined
              dense
              debounce="600"
              v-model="filter"
              placeholder="Buscar"
              style="min-width: 450px; max-width: 600px"
            >
              <template v-slot:append>
                <q-icon name="search" />
              </template>
            </q-input>
          </template>
        </q-table>
      </div>

      <div class="col-6">
        <h1>hola</h1>

        <apexchart
          width="500"
          type="bar"
          :options="options"
          :series="series"
        ></apexchart>
      </div>
    </div>
  </q-page>
</template>

<script>
import { ref } from "vue";
import axios from "axios";
import VueApexCharts from "vue3-apexcharts";
import { defineComponent } from "vue";

export default defineComponent({
  name: "InicioPage",
  setup() {
    return {
      initialPagination: {
        sortBy: "desc",
        descending: false,
        page: 1,
        rowsPerPage: 20,
      },
    };
  },
  mounted() {
    this.get_programas();
  },
  data() {
    return {
      sepa: "cell",
      ds_programas: [],
      ds_productos: [],
      ds_actividades: [],
      ds_subpro: [],
      ds_metas: [],
      filter: "",
      ds_titulo: [
        {
          name: "MICRO_RED",
          align: "left",
          label: "MICRO RED",
          field: "nom_micro_red",
          sortable: true,
          headerStyle: { fontSize: "1.2em" },
          style: { fontSize: "1em" },
        },
        {
          name: "EESS",
          align: "left",
          label: "ESTABLECIMIENTO",
          field: "nom_establecimiento",
          sortable: true,
          headerStyle: { fontSize: "1.2em" },
          style: { fontSize: "1em" },
        },
        {
          name: "META",
          align: "right",
          label: "META FISICA",
          field: "meta_fisica",
          sortable: true,
          headerStyle: { fontSize: "1.2em" },
          style: { fontSize: "1em" },
        },
      ],
      cod_prg: null,
      cod_prd: null,
      cod_act: null,
      cod_sub: null,

      chartOptions: {
        chart: {
          id: "vuechart-example",
        },
        xaxis: {
          categories: [1991, 1992, 1993, 1994, 1995, 1996, 1997, 1998],
        },
      },
      series: [
        {
          name: "Vue Chart",
          data: [30, 40, 45, 50, 49, 60, 70, 81],
        },
      ],
    };
  },
  methods: {
    async get_programas() {
      let url = "http://localhost:3000/api/programas";
      try {
        let datos = await axios.get(url);
        this.ds_programas = await datos.data;
      } catch (error) {
        console.log(error);
      }
    },

    async get_productos() {
      let url = "http://localhost:3000/api/productos";
      try {
        let datos = await axios.get(url + "/" + this.cod_prg["cod_programa"]);
        this.ds_productos = await datos.data;
      } catch (error) {
        console.log(error);
      }
    },

    async get_actividades() {
      let url = "http://localhost:3000/api/actividades";
      try {
        let datos = await axios.get(
          url +
            "/" +
            this.cod_prg["cod_programa"] +
            "/" +
            this.cod_prd["cod_producto"]
        );
        this.ds_actividades = await datos.data;
      } catch (error) {
        console.log(error);
      }
    },

    async get_sub_productos() {
      let url = "http://localhost:3000/api/sub_productos";
      try {
        let datos = await axios.get(
          url +
            "/" +
            this.cod_prg["cod_programa"] +
            "/" +
            this.cod_prd["cod_producto"]
        );
        this.ds_subpro = await datos.data;
      } catch (error) {
        console.log(error);
      }
    },

    async get_metas_subpro() {
      let url = "http://localhost:3000/api/metas_subpro";
      try {
        let datos = await axios.get(
          url +
            "/" +
            this.cod_prg["cod_programa"] +
            "/" +
            this.cod_prd["cod_producto"] +
            "/" +
            this.cod_sub["cod_subfinalidad"]
        );
        this.ds_metas = await datos.data;
      } catch (error) {
        console.log(error);
      }
    },

    updateChart() {
      const max = 90;
      const min = 20;
      const newData = this.series[0].data.map(() => {
        return Math.floor(Math.random() * (max - min + 1)) + min;
      });
      // In the same way, update the series option
      this.series = [
        {
          data: newData,
        },
      ];
    },
  },
});
</script>
