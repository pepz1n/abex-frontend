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
};
</script> 