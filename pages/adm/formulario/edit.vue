<template>
  <v-row no-gutters class="py-10 justify-center">
      <v-col cols="6">
        <h1 v-if="!field.id" class="text-center pb-5">Criar campo do formúlario</h1>
        <h1 v-if="field.id" class="text-center pb-5">Edição de campos do formulário</h1>
      </v-col>
      <v-col cols="8">
        <v-form v-model="valid">
          <v-row no-gutters>
            <v-col cols="12">
              <v-text-field v-model="field.name" v-value="field.name" label="Nome" />
            </v-col>
            <v-col cols="12">
              <v-select
                v-model="field.status"
                :items="statusOptions"
                item-title="text"
                item-value="value"
                label="Status"
                return-object
                single-line
              ></v-select>
            </v-col>
            <v-col cols="12">
              <v-select
                v-model="field.type"
                :items="typeOptions"
                item-title="text"
                item-value="value"
                label="Tipo"
                return-object
                single-line
              ></v-select>
            </v-col>
            <v-col cols="12">
              <v-text-field v-model="field.label" v-value="field.label" label="Label" />
            </v-col>
            <v-col cols="12">
              <v-select
                v-model="field.cols"
                :items="colsOptions"
                item-title="text"
                item-value="value"
                label="Tamanho"
                single-line
              ></v-select>
            </v-col>
          </v-row>
          <v-row>
            <v-col cols="2">
              <v-btn color="info" outlined to="/adm/formulario">
                Cancelar
              </v-btn>
            </v-col>
            <v-col cols="2">
              <v-btn color="error" outlined @click="destroy">
                Deletar
              </v-btn>
            </v-col>
            <v-col cols="8">
              <div class="d-flex justify-end">
                <v-btn color="success" outlined @click="persistir">
                  Salvar
                </v-btn>
              </div>
            </v-col>
          </v-row>
        </v-form>
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
      field: {
        id: null,
        name: null,
        type: null,
        options: null,
        status: null,
        label: null,
        higher: null,
        cols: null
      },
      statusOptions: [
        { text: 'Ativo', value: true },   
        { text: 'Inativo', value: false } 
      ],
      colsOptions: [
        { text: '1 - 8.33%', value: '1' },   
        { text: '2 - 16.67%', value: '2' },   
        { text: '3 - 25%', value: '3' },   
        { text: '4 - 33.33%', value: '4' },   
        { text: '5 - 41,66%', value: '5' },   
        { text: '6 - 50%', value: '6' },   
        { text: '7 - 58.33%', value: '7' },   
        { text: '8 - 66.66%', value: '8' },   
        { text: '9 - 75%', value: '9' },   
        { text: '10 - 83.33%', value: '10' },   
        { text: '11 - 91.66%', value: '11' },   
        { text: '12 - 100%', value: '12' },   
      ],
      typeOptions: [
        { text: 'Texto', value: 'text' },   
      ]
    }
  },

  created() {
    if(this.$route.query &&  this.$route.query.id ){
      this.field.id = this.$route.query.id;
      this.getField(this.field.id)
    }
  },
  methods: {
    async persistir() {
      try {
        let field = {
          id: this.field.id,
          name: this.field.name,
          type: this.field.type.value || this.field.type.value,
          options: this.field.options || {},
          status: this.field.status.value || this.field.status.value,
          label: this.field.label,
          higher: this.field.higher || null,
          cols: this.field.cols || this.field.cols.value
        };
        
        if (!this.field.id) {
          let response = (await axios.post(`${import.meta.env.VITE_API_URL}/field/persist`, field)).data;
          
          if(response.type != 'success') {
            return toast(response.message, {
              autoClose: 1000,
              position: 'bottom-right',
              theme: 'dark'
            });
          }
          
          toast(`Registro criado com sucesso`, {
            autoClose: 1000,
            position: 'bottom-right',
            theme: 'dark'
          });

          this.$router.push('/adm/formulario')

        } else {

          field.id = this.field.id
          let response = (await axios.post(`${import.meta.env.VITE_API_URL}/field/persist/${field.id}`, field)).data;
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

    async destroy() {
      try {
        if (confirm(`Deseja deletar ${this.field.label}?`)) {
          let response = await axios.post(`${import.meta.env.VITE_API_URL}/field/destroy`, {"id": this.field.id});
          toast(`Registro deletado com sucesso`, {
            autoClose: 1000,
            position: 'bottom-right',
            theme: 'dark'
          });
          return this.$router.push('/adm/formulario');
        }
      }
      catch (error) {
        toast(`Ocorreu um erro ao deletar o registro id ${this.field.id}, contate o administrador`, {
          autoClose: 1000,
          position: 'bottom-right',
          theme: 'dark'
        });
      }
    },

    async getField(id) {
      try {
        let field = (await axios.get(`${import.meta.env.VITE_API_URL}/field/${id}/`)).data;

        if(field.type != 'success') {
          return toast(field.message, {
            autoClose: 1000,
            position: 'bottom-right',
            theme: 'dark'
          });  
        }

        return this.field = field.data;
      } catch (error) {
        toast(`Ocorreu um erro ao editar o registro, contate o administrador`, {
          autoClose: 1000,
          position: 'bottom-right',
          theme: 'dark'
        });
      }
    },
  },

}
</script>