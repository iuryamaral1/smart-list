<template>
  <v-container grid-list-md text-xs-center>
    <v-layout row wrap>
      <v-flex xs12 text-center>
        <h1>Gere agora a sua lista smart</h1> 
        <v-main>
          <v-container class="fill-height" fluid>
              <v-row align="center" justify="center">
                <v-col cols="12" sm="8" md="4">
                  <v-text-field label="Gerar URL da lista" placeholder="Nome da sua lista" prepend-icon="mdi-clipboard-list-outline"></v-text-field>
                  <v-btn large color="primary">criar minha lista</v-btn>
                </v-col>
                <v-col cols="9">
                  <v-text-field placeholder="Adicionar item" prepend-icon="mdi-clipboard-list-outline" v-model="itemToAdd"></v-text-field>
                </v-col>
                <v-col cols="3">
                  <v-btn class="mx-2" fab dark color="indigo" v-on:click="addItem()">
                    <v-icon dark>mdi-plus</v-icon>
                  </v-btn>
                </v-col>
                <v-col cols="12" sm="8" md="4">
                  <v-list-item v-for="item in groceryList" :key="item.text" class="{ 'done': item.done }">
                    <v-list-item-content>
                      <v-list-item-title v-text="item.text"></v-list-item-title>
                      <v-icon @click="checkDone(item)">mdi-check</v-icon>
                    </v-list-item-content>
                    <v-list-item-icon @click="removeItem(item)">
                      <v-icon>mdi-close</v-icon>
                    </v-list-item-icon>
                  </v-list-item>
                </v-col>
              </v-row>
          </v-container>
        </v-main>
      </v-flex> 
    </v-layout>
  </v-container>
</template>

<script lang="ts">
  import Vue from 'vue'

  export default Vue.extend({
    name: 'SmartList',
    data: () => {
      return {
        groceryList: [],
        itemToAdd: '',
      }
    },
    methods: {
      addItem() {
        if (this.itemToAdd) {
          const itemToPush = {
            text: this.itemToAdd,
            done: false
          }
          this.groceryList.push(itemToPush);
          this.itemToAdd = '';
        }
      },

      removeItem(item: any) {
        const indexOf = this.groceryList.indexOf(item);
        this.groceryList.splice(indexOf, 1);
      },

      checkDone(item: any) {
        item.done = !item.done;
      }
    }
  })
</script>

<style scoped>
  .done {
    color: olivedrab;
  }
</style>