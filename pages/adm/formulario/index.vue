<template>
  <section>
    <v-row>
      <v-col cols="10">
        <h1 style="color: #1aa5f">Campos do formulário</h1>
      </v-col>
      <v-col cols="2">
        <v-icon color="blue"
          class="mdi-plus mdi text-blue v-btn--icon v-btn--density-default v-btn--size-large v-btn--variant-elevated "
          @click="persist()">
          mdi-plus
        </v-icon>
      </v-col>
      <v-col cols="11">
        <ViewForm :showEdition="true"/>
      </v-col>
    </v-row>
  </section>
</template>
   
<script>
import axios from 'axios';
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';
export default {
  layout: "default",
  name: "defaultfieldsPage",
  data() {
  },
  methods: {
    async persist(field) {
      try {
        // Se você quiser navegar para a página com base no ID do campo
        if (field && field.id) {
          return this.$router.push(`/adm/formulario/edit?id=${field.id}`);
        }

        // Se você quiser navegar para a página com um ID padrão (por exemplo, 1)
        return this.$router.push(`/adm/formulario/edit`);
      } catch (error) {
        toast(`Ocorreu um erro, contate o administrador`, {
          autoClose: 1000,
          position: 'bottom-right',
          theme: 'dark'
        });
      }
    },
  }
};
</script> 