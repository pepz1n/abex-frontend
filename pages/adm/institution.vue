<template>
  <v-container>
    <h1>
      Instituições
    </h1>
    <v-card class="rounded-lg pa-4 mt-2" elevation="10">
      <template v-slot:title>
        <v-row>
          <v-col cols="10">
            <v-text-field
              small
              v-model="search"
              color="primary"
              append-icon="mdi-magnify"
              placeholder="Pesquisar"
              hint="Busque alguma instituição"
              single-line
              variant="outlined"
              rounded
              dense
            >
            </v-text-field>
          </v-col>
          <v-col>
            <v-btn
              class="mt-2 mr-n2"
              elevation="1"
              color="primary"
              small
              icon="mdi-plus"
              @click="newItem"
            >
            </v-btn>
          </v-col>
        </v-row>
      </template>
      <template v-slot:text>
        <template>
          <v-data-table
            height="500"
            :items="items"
            :loading="loadingTable"
            :headers="headers"
            items-per-page-text="Itens por página"
            no-data-text="Nenhum Registro encontrado"
            :search="search"
          >
          </v-data-table>
        </template>
      </template>
    </v-card>
    <template>
      <v-row justify="center">
        <v-dialog
          v-model="dialog"
          persistent
          width="1024"
        >
          <v-card height="700" width="1000">
            <v-tabs
              v-model="tab"
              bg-color="primary"
            >
              <v-tab value="one">Usuário</v-tab>
              <v-tab value="two">Instituição</v-tab>
              <v-tab value="three">Endereço</v-tab>
            </v-tabs>
            <v-card-text>
              <v-container>
                <v-window v-model="tab">
                  <v-window-item value="one">
                    <v-row class="mt-2">
                      <v-col>
                        <v-text-field
                          v-model="requisicao.responsavel.name"
                          variant="outlined"
                          label="Nome do Responsável"
                          placeholder="Nome do Responsável"
                        ></v-text-field>
                      </v-col>
                      <v-col>
                        <v-text-field
                          v-model="requisicao.responsavel.username"
                          variant="outlined"
                          label="Login"
                          placeholder="Login"
                        ></v-text-field>
                      </v-col>
                    </v-row>
                    <v-row>
                      <v-col>
                        <v-text-field
                          v-model="requisicao.responsavel.documentNumber"
                          type="email"
                          variant="outlined"
                          label="Email do Responsável"
                          placeholder="Email do Responsável"
                        ></v-text-field>
                      </v-col>
                    </v-row>
                    <v-row class="mt-n6">
                      <v-col>
                        <v-text-field
                          v-model="requisicao.responsavel.email"
                          type="email"
                          variant="outlined"
                          label="Email do Responsável"
                          placeholder="Email do Responsável"
                        ></v-text-field>
                      </v-col>
                    </v-row>
                    <v-row class="mt-n6">
                      <v-col>
                        <v-text-field
                          variant="outlined"
                          label="Confirme o Email"
                          placeholder="Confirme o Email"
                        ></v-text-field>
                      </v-col>
                    </v-row>
                  </v-window-item>

                  <v-window-item value="two">
                    <v-row>
                      <v-col>
                        <v-text-field
                          v-model="requisicao.instituicao.name"
                          variant="outlined"
                          label="Nome da Instituição"
                          placeholder="Nome da Instituição"
                        ></v-text-field>
                      </v-col>
                    </v-row>
                    <v-row>
                      <v-col>
                        <v-text-field
                          v-model="requisicao.instituicao.documentNumber"
                          variant="outlined"
                          label="CNPJ"
                          placeholder="CNPJ"
                        ></v-text-field>
                      </v-col>
                    </v-row>
                  </v-window-item>

                  <v-window-item value="three">
                    <v-row>
                      <v-col>
                        <v-text-field
                          v-model="requisicao.endereco.country"
                          variant="outlined"
                          label="País"
                          placeholder="País"
                        ></v-text-field>
                      </v-col>
                      <v-col>
                        <v-text-field
                          v-model="requisicao.endereco.state"
                          variant="outlined"
                          label="Estado"
                          placeholder="Estado"
                        ></v-text-field>
                      </v-col>
                    </v-row>
                    <v-row>
                      <v-col>
                        <v-text-field
                          v-model="requisicao.endereco.postalCode"
                          variant="outlined"
                          label="CEP"
                          placeholder="CEP"
                        ></v-text-field>
                      </v-col>
                      <v-col>
                        <v-text-field
                          v-model="requisicao.endereco.city"
                          variant="outlined"
                          label="Cidade"
                          placeholder="Cidade"
                        ></v-text-field>
                      </v-col>
                    </v-row>
                    <v-row>
                      <v-col>
                        <v-text-field
                          v-model="requisicao.endereco.neighborhood"
                          variant="outlined"
                          label="Bairro"
                          placeholder="Bairro"
                        ></v-text-field>
                      </v-col>
                      <v-col>
                        <v-text-field
                          v-model="requisicao.endereco.street"
                          variant="outlined"
                          label="Rua"
                          placeholder="Rua"
                        ></v-text-field>
                      </v-col>
                    </v-row>
                  </v-window-item>
                </v-window>
              </v-container>
            </v-card-text>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="red"
                variant="tonal"
                @click="dialog = false"
              >
                Close
              </v-btn>
              <v-btn
                v-if="tab === 2"
                color="blue-darken-1"
                variant="tonal"
                @click="persist"
              >
                Save
              </v-btn>
              <v-btn
                size="large"
                v-else
                @click="tab = tab + 1"
                color="blue-darken-1"
                variant="tonal"
                icon="mdi-arrow-right-thick"
                fab
              ></v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-row>
    </template>
  </v-container>
</template>

<script>
  export default {
    name: 'institutionPage',
    data() {
      return {
        search: null,
        requisicao: {
          responsavel: {
            name: null,
            email: null,
            username: null,
            documentNumber: null,
          },
          instituicao: {
            name: null,
            documentNumber: null,
          },
          endereco: {
            country: null,
            state: null,
            city: null,
            neighborhood: null,
            street: null,
            postalCode: null,
          }
        },
        dialog: false,
        items: [],
        tab: 0,
        loadingTable: false,
        headers: [
          {
            title: 'ID',
            align: 'start',
            key: 'id'
          },
          {
            title: 'Nome',
            align: 'start',
            key: 'name'
          },
        ]
      }
    },

    async created () {
      await this.getAll();
    },

    methods: {
      async getAll() {
        try {
          this.items = await $fetch('http://localhost:3333/institution').then(a => JSON.parse(JSON.stringify(a)).data);
          console.log(this.items);
        } catch (error) {
          console.log(error);
        }
      },

      async persist() {
        try {
          this.response = await $fetch('http://localhost:3333/institution/persist', {
            method: 'post',
            body: this.requisicao,
          });
          await this.getAll()
        } catch (error) {
          console.log(error);
        }
      },

      newItem () {
        this.dialog = true;
      }
    },

  }
</script>
