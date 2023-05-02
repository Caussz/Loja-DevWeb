<script setup>
import { ref } from 'vue';
import { produtos } from './utils/produtos';

const carrinho = ref([]);
const totalProdutos = ref(0);
const totalPreco = ref(0);
const mostrarCarrinho = ref(true);
const mostrar = ref(true)

const novoItem = ref({
  id: 0,
  nome: '',
  preco: 0,
});

function adicionar(index) {
  const produtoSelecionado = produtos[index];
  novoItem.value.id = produtoSelecionado.id;
  novoItem.value.nome = produtoSelecionado.nome;
  novoItem.value.preco = produtoSelecionado.preco;
  carrinho.value.push({ ...novoItem.value });
  novoItem.value.id = 0;
  novoItem.value.nome = '';
  novoItem.value.preco = 0;
  calcularTotal();
}

function remover(index) {
  carrinho.value.splice(index, 1);
  calcularTotal();
}

function calcularTotal() {
  totalProdutos.value = carrinho.value.length;
  totalPreco.value = carrinho.value.reduce((total, item) => total + item.preco, 0);
}
</script>
<template>
  <div class="container" v-if="mostrarCarrinho">
    <div class="header">
      <h1>Produtos</h1>
      <button class="cart-button" @click="mostrarCarrinho = false">
        Carrinho ({{ totalProdutos }})
      </button>
    </div>
    <div class="content">
      <div class="produtos">
        <div v-for="(produto, index) of produtos" :key="produto.id" class="produto">
          <div class="produto-info">
            <h2>{{ produto.nome }}</h2>
            <p>{{ produto.descricao }}</p>
            <p>R$ {{ produto.preco.toFixed(2) }}</p>
          </div>
          <button class="add-button" @click="adicionar(index)">Adicionar</button>
        </div>
      </div>
    </div>
  </div>
  <div class="carrinho" v-else>
    <div class="carrinho-header">
      <h2>Carrinho ({{ totalProdutos }})</h2>
      <button class="close-button" @click="mostrarCarrinho = true">
        Fechar
      </button>
    </div>
    <div class="carrinho-items">
      <div v-if="carrinho.length === 0">
        <p>Nenhum produto no carrinho.</p>
      </div>
      <div v-else>
        <div v-for="(item, index) of carrinho" :key="item.id" class="carrinho-item">
          <div class="carrinho-item-info">
            <h3>{{ item.nome }}</h3>
            <p>R$ {{ item.preco.toFixed(2) }}</p>
          </div>
          <button class="remove-button" @click="remover(index)">Remover</button>
        </div>
        <div class="carrinho-total">
          <p>Total: R$ {{ totalPreco.toFixed(2) }}</p>
        </div>
      </div>
    </div>
  </div>
</template>


<style scoped>
li{
  background-color: aqua;
}
</style>
