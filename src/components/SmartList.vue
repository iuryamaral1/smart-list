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
                <v-col cols="12">
                 <v-chip class="ma-2" label>
                   Valor Total: R$ {{ getTotalPrice() }}
                 </v-chip>
                 <v-tabs
                    v-model="tab"
                    background-color="transparent"
                    color="basil"
                    grow
                  >
                    <v-tab
                      v-for="item in status"
                      :key="item"
                    >
                      {{ item }}
                    </v-tab>
                  </v-tabs>
                </v-col>
                <v-col cols="12">
                  <v-tabs-items v-model="tab">
                    <v-tab-item v-for="item in status" :key="item">
                      <v-card color="basil" flat>
                        
                        <v-card-text class="align-items-center" v-if="!filterGroceryList(item) || !filterGroceryList(item).length">
                          NENHUM ITEM ADICIONADO À LISTA AINDA
                        </v-card-text>
                        
                        <v-card-text v-if="filterGroceryList(item) && filterGroceryList(item).length" class="d-inline-flex justify-space-between align-items-center">
                          <v-col cols="3">
                            PRODUTO
                          </v-col>
                          <v-col cols="3">
                            PREÇO
                          </v-col>
                          <v-col cols="3">
                            QUANTIDADE
                          </v-col>
                          <v-col cols="3">
                            AÇÕES                          
                          </v-col>
                        </v-card-text>

                        <v-card-text v-for="k in filterGroceryList(item)" :key="k.text" class="d-inline-flex justify-space-between align-items-center text-card" v-bind:class="{ 'done': k.done }">
                          
                          <v-col cols="3" class="align-self-center">
                            {{ k.text }}
                          </v-col>
                          <v-col cols="3">
                            <v-text-field placeholder="R$ 1,00" v-model="k.price"></v-text-field>
                          </v-col>
                          <v-col cols="3">
                            <v-text-field placeholder="QUANTIDADE" v-model="k.ammount"></v-text-field>
                          </v-col>
                          <v-col cols="3" class="d-inline-flex justify-center align-items-center">
                            <v-icon v-if="!k.done" v-on:click="checkDone(k)" class="pr-2" v-bind:class="{ 'icon-done': k.done }">mdi-check</v-icon>
                            <v-icon v-on:click="removeItem(k)" v-bind:class="{ 'icon-done': k.done }">mdi-close</v-icon>
                          </v-col>
                        
                        </v-card-text>

                      </v-card>
                    </v-tab-item>
                  </v-tabs-items>
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
        status: ['LISTA', 'CARRINHO'],
        tab: null,
        totalPrice: 0,
      }
    },
    methods: {
      addItem() {
        if (this.itemToAdd) {
          const itemToPush = {
            text: this.itemToAdd.toUpperCase(),
            price: 0,
            ammount: 1,
            done: false
          }
          this.groceryList.push(itemToPush);
          this.itemToAdd = '';
        }
      },

      removeItem(item: any) {
        if (item.done) {
          this.checkDone(item);
          if (item.price) {
            this.totalPrice = this.totalPrice - item.price;
          }
        } else {
          const indexOf = this.groceryList.indexOf(item);
          this.groceryList.splice(indexOf, 1);
        }
      },

      checkDone(item: any) {
        item.done = !item.done;
      },

      filterGroceryList(item: string) {
        if (item === 'LISTA') {
          const result = this.groceryList.filter(k => k.done === false);
          return result;
        }

        if (item === 'CARRINHO') {
          return this.groceryList.filter(k => k.done === true);
        }
      },

      getTotalPrice(): number {
        let sum = 0;
        this.groceryList.filter(product => product.done)
            .forEach(prod => {
              sum = sum + (prod.price * prod.ammount);
            });
        return sum;
      }
    }
  })
</script>

<style scoped>
  .text-card {
    padding: 0;
    border: 1px ridge olive;
    border-radius: 5px;
    margin-bottom: 5px;
  }
</style>