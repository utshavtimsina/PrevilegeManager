<template>
  <v-app>

    <v-content>
      <PrevilegeManager/>
    </v-content>
  </v-app>
</template>

<script>
import PrevilegeManager from './components/PrevilegeManager.vue';

export default {
  name: 'App',

  components: {
   PrevilegeManager,
  },

  data: () => ({
    //
  }),
};
</script>
