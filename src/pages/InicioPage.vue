<template>
  <q-page>
    <div class="q-pa-md shadow-5">
      <div class="row">
        <div class="col">
          <q-select
            outlined
            v-model="cod_prg"
            :options="ds_programas"
            stack-label
            option-label="programa"
            option-value="cod_programa"
            label="Programas"
            style="min-width: 500px; max-width: 600px"
            @update:model-value="get_productos()"
          />
        </div>
      </div>

      <div class="row">
        <div class="col">
          <q-select
            outlined
            v-model="cod_prd"
            :options="ds_productos"
            option-label="producto"
            option-value="cod_producto"
            label="Productos"
            style="min-width: 500px; max-width: 600px"
            @update:model-value="get_actividades()"
          />
        </div>
      </div>

      <div class="row">
        <div class="col">
          <q-select
            outlined
            v-model="cod_act"
            :options="ds_actividades"
            option-label="actividad"
            option-value="cod_actividad"
            label="Actividades"
            style="min-width: 500px; max-width: 600px"
            @update:model-value="get_sub_productos()"
          />
        </div>
      </div>

      <div class="row">
        <div class="col">
          <q-select
            outlined
            v-model="cod_sub"
            :options="ds_subpro"
            option-label="sub_finalidad"
            option-value="cod_subfinalidad"
            label="Sub Productos"
            style="min-width: 500px; max-width: 600px"
            @update:model-value="get_rep_prg_fte()"
          />
        </div>
      </div>
    </div>
  </q-page>
</template>

<script>
import axios from "axios";
import { defineComponent } from "vue";

export default defineComponent({
  name: "InicioPage",
  setup() {
    return {};
  },
  mounted() {
    this.get_programas();
  },
  data() {
    return {
      ds_programas: [],
      ds_productos: [],
      ds_actividades: [],
      ds_subpro: [],
      cod_prg: null,
      cod_prd: null,
      cod_act: null,
      cod_sub: null,
    };
  },
  methods: {
    async get_programas() {
      let url = "http://localhost:3000/programas";
      try {
        let datos = await axios.get(url);
        this.ds_programas = await datos.data;
      } catch (error) {
        console.log(error);
      }
    },

    async get_productos() {
      let url = "http://localhost:3000/productos";
      try {
        let datos = await axios.get(url + "/" + this.cod_prg["cod_programa"]);
        this.ds_productos = await datos.data;
      } catch (error) {
        console.log(error);
      }
    },

    async get_actividades() {
      let url = "http://localhost:3000/actividades";
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
      let url = "http://localhost:3000/sub_productos";
      try {
        let datos = await axios.get(
          url +
            "/" +
            this.cod_prg["cod_programa"] +
            "/" +
            this.cod_prd["cod_producto"] +
            "/" +
            this.cod_act["cod_actividad"]
        );
        this.ds_subpro = await datos.data;
      } catch (error) {
        console.log(error);
      }
    },
  },
});
</script>
