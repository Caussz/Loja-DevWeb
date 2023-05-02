<script setup>
  import { ref } from 'vue';
  import {produtos} from './utils/produtos';
  const carrinho = ref([]);
  const precos = ref()
  const mostrarC = ref(false)
  const itemc = ref([])

  

  const novoItem = ref({
    id: 0,
    produto: '',
    preco: 0
  })

 function adicionar(id){
    novoItem.value.id = produtos[id].id;
    novoItem.value.produto = produtos[id].nome;
    novoItem.value.preco = produtos[id].preco;
    carrinho.value.push({...novoItem.value})
    novoItem.value.id = 0;
    novoItem.value.produto = '';
    novoItem.value.preco = 0;
    console.log(id)
  }

  function remover(index) {
    carrinho.value.splice(index, 1)
  }


  for (precos.value of carrinho.value) {
    console.log(precos)
    let totals = precos.value.preco += precos.value.preco
    console.log(totals)
  }

</script>
<template>
  <input type="text" v-model="novoItem.produto">
  <button @click="adicionar()">Adicionar</button>
  <button @click="remover">remover</button>
  <ul>
    <li v-for="(item, index) of produtos"> <p @click="adicionar(index)">add</p> <p @click="remover(index)">remover</p>{{ JSON.stringify(item.nome) }}</li>
  </ul>
  <button @click="mostrarC = true">CArrinho</button>

  <div v-if="mostrarC">
    <li v-for="(itemc, index) of carrinho"> <p @click="adicionar(index)">add</p> <p @click="remover(index)">remover</p>{{ JSON.stringify(itemc.nome) }}</li>
    <button @click="mostrarC = false">produtos</button>

  </div>
</template>


<style scoped>
li{
  background-color: aqua;
}
</style>
