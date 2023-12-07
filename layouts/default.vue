<template>
  <v-card>
    <v-layout>
      <v-navigation-drawer class="custom-drawer h-100" style="width: clamp(287px, 19vw, 400px) !important; position: relative !important; min-height: 100vh !important;"  permanent color="#0040FF" theme="dark">
        <h1 class="title-nav px-5 pt-5" style="font-size: 21px; letter-spacing: 0;word-spacing: 0px; ">Sistema de proteção crianças e adolecentes</h1>
        <v-list color="transparent">
          <li >
            {{ item }}
          </li>
<<<<<<< HEAD
          <v-list-item @click="redirect(item.to)" v-for="(item, index) in itemsMenu" :key="index" :prepend-icon="item.icon" :title="item.text"></v-list-item>
=======
          <v-list-item class="text-grey-lighten-5" :to="item.to" v-for="(item, index) in itemsMenu" :key="index" :prepend-icon="item.icon" :title="item.text"></v-list-item>
>>>>>>> bea0c143680d5df67b66831e4b34030cae5985a0
        </v-list>
        <div class="mt-auto">
          <v-btn class="p-0 bg-transparent"  style="box-shadow: none;position: inherit; margin-bottom: 1%;" @click="logout">
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
<<<<<<< HEAD
            'permission': 'user'
=======
            'permission': 'user',
            'to': '/'
>>>>>>> bea0c143680d5df67b66831e4b34030cae5985a0
          },
          {
            'icon': "mdi-file" ,
            'text': "Formulário",
<<<<<<< HEAD
            'permission': 'user'
=======
            'permission': 'user',
            'to': '/adm/formulario'
>>>>>>> bea0c143680d5df67b66831e4b34030cae5985a0
          },
          {
            'icon': "mdi-finance" ,
            'text': "Gráficos",
<<<<<<< HEAD
            'permission': 'admin'
=======
            'permission': 'admin',
            'to': '/'
>>>>>>> bea0c143680d5df67b66831e4b34030cae5985a0
          },
          {
            'icon': "mdi-domain" ,
            'text': "Instituições",
            'permission': 'admin',
<<<<<<< HEAD
            'to': '/adm/institution'
          },
          {
            'icon': "mdi-email-search" ,
            'text': "Consulta de denúncias",
            'permission': 'user'
=======
            'to': '/'
          },
          {
            'icon': "mdi-email-search" ,
            'text': "Denúncias",
            'permission': 'user',
            'to': '/denuncias'
>>>>>>> bea0c143680d5df67b66831e4b34030cae5985a0
          },
      ],
    }),

    created() {

    },

    methods: {
      async getPermissionUser(user) {
        return 'admin';
      },

      redirect (to) {
        this.$router.push(to)
      },

      logout() {
        localStorage.clear();
        this.$router.push({name: 'index'});
      }
    },

    watch: {
      group () {
        this.drawer = false
      },
    },
  }
</script>
