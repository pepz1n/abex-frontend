<template>
  <v-card>
    <v-layout>
      <v-main style="min-height: 100vh;" class="text-left w-auto pt-3 pb-0 pr-0 pl-5">
        <slot />
      </v-main>
    </v-layout>
  </v-card>

</template>

<style>
.sidebar {
  background-color: aliceblue;
}
.custom-drawer .v-navigation-drawer__content
{
  display: flex;
  flex-direction: column;
  height: 100vh;
}

</style>

<script>
  export default {
    name: 'loginLayout',
  }
</script>