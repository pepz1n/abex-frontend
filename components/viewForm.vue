<template>
  <v-section>
    <v-row>
      <ViewField v-for="item in fields" :showEdition="showEdition" :field="item" />
      <v-col cols="12" v-if="showBtn">
        <v-btn :to="()=>this.$router.push('/')" class="warning">Cancelar</v-btn>
        <v-btn @click="persist.persist(denunciation)" class="success">Salvar</v-btn>
      </v-col>
    </v-row>
  </v-section>
</template>

<script>
import axios from 'axios';
import { ref } from 'vue';
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';

export default {
  data() {
    return {
      fields: []
    };
  },
  props: {
    showBtn: {
      type: Boolean,
      default: false,
    },
    showEdition: {
      type: Boolean,
      default: false,
    },
    denunciation: {
      type: Object,
      default: {}
    },
    fieldsForm: {
      type: Array,
      default: []
    },
    persist: { 
      default: {persist: ()=>{}}
    }
  },
  
  methods: {
    async getFields() {
      try {
        let response = await axios.get(`${import.meta.env.VITE_API_URL}/field`);
        this.fields = response.data.data;
      } 
      catch (error) {
        console.log(error)
        toast(`Ocorreu um erro ao carregar a página, contate o administrador`, {
          autoClose: 1000,
          position: 'bottom-right',
          theme: 'dark'
        });
      }
    },
  },
  mounted() {
    this.getFields() 
  }, 
};
</script>
