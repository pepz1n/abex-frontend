<template>
  <v-card>
    <v-layout>
      <v-navigation-drawer class="custom-drawer h-100" style="width: clamp(287px, 19vw, 400px) !important; position: relative !important; min-height: 100vh !important;"  permanent color="#0040FF" theme="dark">
        <h1 class="title-nav px-5 pt-5" style="font-size: 21px; letter-spacing: 0;word-spacing: 0px; ">Sistema de proteção crianças e adolecentes</h1>
        <v-list color="transparent">
          <li >
            {{ item }}
          </li>
          <v-list-item class="text-grey-lighten-5" :to="item.to" v-for="(item, index) in itemsMenu" :key="index" :prepend-icon="item.icon" :title="item.text"></v-list-item>
        </v-list>
        <div class="mt-auto">
          <v-btn class="p-0 bg-transparent"  style="box-shadow: none;position: inherit; margin-bottom: 1%;">
            <v-icon class="mdi mdi-logout" style="transform: rotate(180deg) translateX(4px);"></v-icon>
            Sair
          </v-btn>
        </div>
      </v-navigation-drawer>  
      <v-main style="min-height: 100vh;" class="text-left w-auto pt-3 pb-0 pr-0 pl-5">
        <slot />
      </v-main>
    </v-layout>
  </v-card>

</template>

<style scoped>
.sidebar {
  background-color: aliceblue;
}
.custom-drawer .v-navigation-drawer__content
{
  display: flex;
  flex-direction: column;
  height: 100vh;
}

.v-list-item-title, a.v-list-item.text-transparent{
  color: #fff !important;
  background-color: red;
}

</style>

<script>
  export default {
    data: () => ({
      drawer: false,
      group: null,
      itemsMenu: [
          {
            'icon': "mdi-home" ,
            'text': "Inicial",
            'permission': 'user',
            'to': '/'
          },
          {
            'icon': "mdi-file" ,
            'text': "Formulário",
            'permission': 'user',
            'to': '/adm/formulario'
          },
          {
            'icon': "mdi-finance" ,
            'text': "Gráficos",
            'permission': 'admin',
            'to': '/'
          },
          {
            'icon': "mdi-domain" ,
            'text': "Instituições",
            'permission': 'admin',
            'to': '/'
          },
          {
            'icon': "mdi-email-search" ,
            'text': "Denúncias",
            'permission': 'user',
            'to': '/denuncias'
          },
      ],
    }),

    created() {
      
    },

    methods: {
      async getPermissionUser(user) {
        return 'admin';
      }

      // asyn
    },

    watch: {
      group () {
        this.drawer = false
      },
    },
  }
</script>
