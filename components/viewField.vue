<template>
  <v-col v-if="field.status" :cols="field.cols" :key="field.id">
    <v-text-field :label="field.label" :type="field.type" :name="field.name"
      @contextmenu.prevent="showEdition ? persist(field) : () => { }"></v-text-field>
  </v-col>
</template>


<script>
import axios from 'axios';
import { ref } from 'vue';
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';

export default {
  props: {
    field: {
      type: Object,
      default: {},
    },
    showEdition: {
      type: Boolean,
      default: false,
    }
  },
  methods: {
    // async destroy(field) {
    //   try {
    //     if (confirm(`Deseja deletar ${field.field}?`)) {
    //       await axios.delete(`${import.meta.env.VITE_API_URL}/fields/${field.id}`);
    //       toast(`Registro deletado com sucesso`, {
    //         autoClose: 1000,
    //         position: 'bottom-right',
    //         theme: 'dark'
    //       });
    //       return this.getfields();
    //     }
    //   } catch (error) {
    //     toast(`Ocorreu um erro ao deletar o registro id ${field.id}, contate o administrador`, {
    //       autoClose: 1000,
    //       position: 'bottom-right',
    //       theme: 'dark'
    //     });
    //   }
    // },
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
  },
};
</script>
