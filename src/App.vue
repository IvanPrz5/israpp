<template>
  <v-app>
    <v-main>
      <!-- <FormBrutoNeto /> -->
      <FormNetoBruto />
    </v-main>
  </v-app>
</template>

<script>
/* import FormBrutoNeto from "./components/FormBrutoNeto.vue" */
import FormNetoBruto from "./components/FormNetoBruto.vue";

export default {
  name: 'App',
  components: {
    /* FormBrutoNeto, */
    FormNetoBruto
},

  data: () => ({
    //
  }),
};
</script>
