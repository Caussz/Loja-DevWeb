<script setup>
  import { ref } from 'vue'

  const carrinho = ref([]);

  const produtos = [
    {
        id: 1,
        nome: 'Camiseta',
        preco: 49.90
    },
    {
        id: 2,
        nome: 'Calça',
        preco: 99.90
    },
    {
        id: 3,
        nome: 'Meia',
        preco: 9.90
    },
    {
        id: 4,
        nome: 'Sapato',
        preco: 199.90
    },
    {
        id: 5,
        nome: 'Boné',
        preco: 29.90
    },
    {
        id: 6,
        nome: 'Óculos',
        preco: 99.90
    },
    {
        id: 7,
        nome: 'Relógio',
        preco: 299.90
    },
    {
        id: 8,
        nome: 'Bermuda',
        preco: 79.90
    },
    {
        id: 9,
        nome: 'Cueca',
        preco: 19.90
    },
    {
        id: 10,
        nome: 'Meia',
        preco: 9.90
    }
]

  const novoItem = ref({
    id: 0,
    produto: '',
    preco: 0
  })

 function adicionar(id){
    novoItem.value.id = produtos[id].id;
    novoItem.value.produto = produtos[id].nome;
    novoItem.value.preco = produtos[id].preco;
    carrinho.value.push(novoItem.value)
    novoItem.value.id = 0;
    novoItem.value.produto = '';
    novoItem.value.preco = 0;
    console.log(id)
  }

  function remover(index) {
    carrinho.value.splice(index, 1)
  }
</script>
<template>
  <input type="text" v-model="novoItem.produto">
  <button @click="adicionar(2)">Adicionar</button>
  <button @click="remover">remover</button>
  <ul>
    <li v-for="(item, index) of produtos" @click="adicionar(index)">{{ JSON.stringify(item.nome) }}</li>
  </ul>
  {{ carrinho }}
</template>


<style scoped>
li{
  background-color: aqua;
}
</style>
