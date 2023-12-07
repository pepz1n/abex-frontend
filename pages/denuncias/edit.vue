<template>
  <v-row no-gutters class="py-10 justify-center">
    <v-col cols="6">
      <h1 v-if="!denunciation.id" class="text-center pb-5">Registrar nova denuncia</h1>
      <h1 v-if="denunciation.id" class="text-center pb-5">Edição denuncia</h1>
    </v-col>
    <v-col cols="10">
      <ViewForm :denunciation="denunciation" :fieldsForm="fields" :persist="{persist}" :showBtn="true"></ViewForm>
    </v-col>
  </v-row>
</template> 

<script>
import axios from 'axios';
import { toast } from 'vue3-toastify';
import 'vue3-toastify/dist/index.css';
export default {
  layout: 'default',
  name: 'editPage',
  data() {
    return {
      denunciation: {
        id: null,
        date: null,
        status: null,  
        createdUserId: null,
        responsibleUserId: null
      },
      fields: {}
    }
  },

  created() {
    if (this.$route.query && this.$route.query.id) {
      this.denunciation.id = this.$route.query.id;
      this.getDenunciation(this.denunciation.id)
    }
  },
  methods: {
    async persist() {
      try {
        let denunciation = {
          id: this.denunciation.id,
          date: new Date(),
          status: 1,
          createdUserId: 2,
          responsibleUserId: 2
        };
        
        if (!this.denunciation.id) {
          let response = (await axios.post(`${import.meta.env.VITE_API_URL}/denunciation/persist`, denunciation)).data;

          if (response.type != 'success') {
            return toast(response.message, {
              autoClose: 1000,
              position: 'bottom-right',
              theme: 'dark'
            });
          }

          // SET FIELDS_RESPONSE
          let fields = this.getFields();

          await fields.forEach( e => {

          })

          toast(`Registro criado com sucesso`, {
            autoClose: 1000,
            position: 'bottom-right',
            theme: 'dark'
          });

          this.$router.push('/adm/formulario')

        } else {

          denunciation.id = this.denunciation.id
          let response = (await axios.post(`${import.meta.env.VITE_API_URL}/denunciation/persist/${denunciation.id}`, denunciation)).data;
          toast(`Registro atualizado com sucesso`, {
            autoClose: 1000,
            position: 'bottom-right',
            theme: 'dark'
          });

          return this.$router.push('/adm/formulario');
        }
      } catch (error) {
        toast(`Ocorreu um erro no cadastro, contate o administrador`, {
          autoClose: 1000,
          position: 'bottom-right',
          theme: 'dark'
        })
      }
    },

    
    async getDenunciation(id) {
      try {
        let denunciation = (await axios.get(`${import.meta.env.VITE_API_URL}/denunciation/${id}`)).data;

        if (denunciation.type != 'success') {
          return toast(denunciation.message, {
            autoClose: 1000,
            position: 'bottom-right',
            theme: 'dark'
          });
        }
        denunciation = denunciation.data
        
        denunciation.responsible_person = await this.getPerson(denunciation.responsibleUserId)
        denunciation.responsible_person.institution = await this.getInstitution(denunciation.responsible_person.institutionId)
        
        denunciation.created_person = await this.getPerson(denunciation.createdUserId)
        denunciation.created_person.institution = await this.getInstitution(denunciation.created_person.institutionId)
        console.log(denunciation)
        this.getFieldResponse(denunciation.id)

        // atualiza a global
        this.denunciation = denunciation
        
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

    async getFieldResponse(idDenunciation) {
      let fields = await this.getFields()
      let fieldsResponse = await axios.get(`${import.meta.env.VITE_API_URL}/response-field/denunciation/${idDenunciation}`);
      console.log(fieldsResponse);
      fields.forEach(async e => {
      })
    },
    
    async setFieldResponse(idDenunciation) {
      let fields = await this.getFields()
      let fieldsResponse = await axios.get(`${import.meta.env.VITE_API_URL}/response-field/denunciation/${idDenunciation}`);
      console.log(fieldsResponse);
      fields.forEach(async e => {
      })
    },

    async getFields() {
      try {
        let response = await axios.get(`${import.meta.env.VITE_API_URL}/field`);
        return response.data.data;
      }
      catch (error) {
        toast(`Ocorreu um erro ao carregar a página, contate o administrador`, {
          autoClose: 1000,
          position: 'bottom-right',
          theme: 'dark'
        });
      }
    },
  },

}
</script>