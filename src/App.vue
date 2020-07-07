<template>
  <v-app>
    <SmartList/>
  </v-app>
</template>

<script lang="ts">
import Vue from 'vue';
import SmartList from './components/SmartList.vue';

export default Vue.extend({
  name: 'App',

  components: {
    SmartList,
  },

  data: () => ({
    //
  }),
});
</script>
