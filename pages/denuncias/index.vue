<template>
  <section>
    <v-row>
      <v-col cols="10">
        <h1 style="color: #1aa5f">Denúncias</h1>
      </v-col>
      <v-col cols="7">
        <v-btn to="/adm/formulario">Editar formulário</v-btn>
      </v-col>
      <v-col cols="5">
        <v-btn @click="persist()">Adicionar denuncia</v-btn>
      </v-col>
    </v-row>
  </section>
  <v-container class="pt-10">
    <v-row>
      <v-col cols="10">

        <v-data-table
          :headers="headers"
          :items="denunciations" 
          :search="search"
          items-per-page-text="Itens por página"
          show-current-page="true"
          theme="light"
          >
          <template v-slot:item.actions="{ item }">
            <v-icon small color="info" class="mr-2" @click="toDenunciation(item.id)">
              mdi-eye
            </v-icon> 
            <!-- <v-icon small color="yellow" class="mr-2" @click="destroy(item)">
              mdi-pencil
            </v-icon> -->
          </template>
        </v-data-table>
      </v-col>
    </v-row>
  </v-container>
</template>
   
<script>
import axios from 'axios';
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';
export default {
  layout: "default",
  name: "defaultDenunciationPage",
  data() {
    return {
      search: "",
      denunciations: [],
      headers: [
        { title: "ID", key: "id" },
        { title: "Data", key: "date" },
        { title: "Responsável", key: "responsible_person.name" },
        { title: "Instituição", key: "responsible_person.institution.name" },
        { title: "", key: "actions" },
      ],
    };
  },
  created() {
    this.getDenunciations();
  },
  methods: {
    // async destroy(denunciation) {
    //   try {
    //     if (confirm(`Deseja deletar ${denunciation.denunciation}?`)) {
    //       await axios.delete(`${import.meta.env.VITE_API_URL}/denunciation/${denunciation.id}`);
    //       toast(`Registro deletado com sucesso`, {
    //         autoClose: 1000,
    //         position: 'bottom-right',
    //         theme: 'dark'
    //       });
    //       return this.getDenunciations();
    //     }
    //   } catch (error) {
    //     toast(`Ocorreu um erro ao deletar o registro id ${denunciation.id}, contate o administrador`, {
    //       autoClose: 1000,
    //       position: 'bottom-right',
    //       theme: 'dark'
    //     });
    //   }
    // },

    async persist(denunciation) {
      try {
        // Se você quiser navegar para a página com base no ID do campo
        if (denunciation && denunciation.id) {
          return this.$router.push(`/denuncias/edit?id=${denunciation.id}`);
        }
        
        // Se você quiser navegar para a página com um ID padrão (por exemplo, 1)
        return this.$router.push(`/denuncias/edit`);
      } catch (error) {
        toast(`Ocorreu um erro, contate o administrador`, {
          autoClose: 1000,
          position: 'bottom-right',
          theme: 'dark'
        });
      }
    },

    async getDenunciations(id) {
      try {
        let denunciations = (await axios.get(`${import.meta.env.VITE_API_URL}/denunciation`)).data;

        if (denunciations.type != 'success') {
          return toast(denunciations.message, {
            autoClose: 1000,
            position: 'bottom-right',
            theme: 'dark'
          });
        }

        this.denunciations = [];

          denunciations.data.forEach( async (denunciation) => {
          denunciation.responsible_person = await this.getPerson(denunciation.responsibleUserId)
          denunciation.responsible_person.institution = await this.getInstitution(denunciation.responsible_person.institutionId)
          
          denunciation.created_person = await this.getPerson(denunciation.createdUserId)
          denunciation.created_person.institution = await this.getInstitution(denunciation.created_person.institutionId)
          this.denunciations.push(denunciation);
        });

        return this.denunciations;
      } catch (error) {
        toast(`Ocorreu um erro ao editar o registro, contate o administrador`, {
          autoClose: 1000,
          position: 'bottom-right',
          theme: 'dark'
        });
      }
    },
    
    async getPerson(id) {
      try {
        let person = (await axios.get(`${import.meta.env.VITE_API_URL}/person-account/${id}`)).data;

        if (person.type != 'success') {
          return toast(person.message, {
            autoClose: 1000,
            position: 'bottom-right',
            theme: 'dark'
          });
        }

        return person.data;
      } catch (error) {
        toast(`Ocorreu um erro ao editar o registro, contate o administrador`, {
          autoClose: 1000,
          position: 'bottom-right',
          theme: 'dark'
        });
      }
    },
    async getInstitution(id) {
      try {
        let institution = (await axios.get(`${import.meta.env.VITE_API_URL}/institution/${id}`)).data;

        if (institution.type != 'success') {
          return toast(institution.message, {
            autoClose: 1000,
            position: 'bottom-right',
            theme: 'dark'
          });
        }

        return institution.data;
      } catch (error) {
        toast(`Ocorreu um erro ao editar o registro, contate o administrador`, {
          autoClose: 1000,
          position: 'bottom-right',
          theme: 'dark'
        });
      }
    },

    async toDenunciation (id) {
      this.$router.push('/denuncias/edit?id='+id) 
    }

  },
};
</script> 