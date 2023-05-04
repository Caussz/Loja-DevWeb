<script setup>
import { ref } from "vue";
import { produtos } from "./utils/produtos";

const carrinho = ref([]);
const totalProdutos = ref(0);
const totalPreco = ref(0);
const mostrarCarrinho = ref(true);
const mostrar = ref(false);
const erro0 = ref(false)
const sucess = ref(false)
const novoItem = ref({
  id: 0,
  nome: "",
  preco: 0,
  img: "",
  desc: "",
});

function adicionar(id) {
  const produtoSelecionado = produtos.find((produto) => produto.id === id);
  novoItem.value.id = produtoSelecionado.id;
  novoItem.value.nome = produtoSelecionado.nome;
  novoItem.value.preco = produtoSelecionado.preco;
  novoItem.value.img = produtoSelecionado.img;
  novoItem.value.desc = produtoSelecionado.desc;
  carrinho.value.push({ ...novoItem.value });
  novoItem.value.id = 0;
  novoItem.value.nome = "";
  novoItem.value.preco = 0;
  sucess.value = true
  calcularTotal();
}

function limparCarrinho() {
  carrinho.value = [];
  calcularTotal();
}

function verMais(index) {
  const produtoSelecionado = produtos[index];
  novoItem.value.id = produtoSelecionado.id;
  novoItem.value.nome = produtoSelecionado.nome;
  novoItem.value.preco = produtoSelecionado.preco;
  novoItem.value.img = produtoSelecionado.img;
  novoItem.value.desc = produtoSelecionado.desc;
  mostrar.value = !mostrar.value;
}

function remover(index) {
  carrinho.value.splice(index, 1);
  if (carrinho.value.length == 0) erro0.value = true;
  calcularTotal();
}

function calcularTotal() {
  totalProdutos.value = carrinho.value.length;
  totalPreco.value = carrinho.value.reduce((total, item) => total + item.preco, 0);
}

function filtrarPorCategoria(categoria) {
  return produtos.filter((produto) => produto.categoria === categoria);
}

function buscarPorNome(nome) {
  return produtos.filter((produto) => produto.nome.toLowerCase().includes(nome.toLowerCase()));
}

function ordenarPorPreco() {
  produtos.sort((a, b) => a.preco - b.preco);
}

</script>
<template>
  <header v-if="mostrarCarrinho && mostrar == false">
    <h1>Produtos</h1>
    <button class="cart-button" @click="mostrarCarrinho = false">
     Meu carrinho ({{ totalProdutos }})
    </button>
  </header>

  <div v-if="sucess" class="alert-sucess">
    <span class="closebtn" @click="sucess = false">&times;</span>
    <strong>Sucesso! Item adicionado ao carrinho...</strong> {{ text }}
  </div>
  <div v-if="mostrar">
    <div v-if="erro0" class="alert">
      <span class="closebtn" @click="erro0 = false">&times;</span>
      <strong>Erro! Primeiro adicione um item...</strong> {{ text }}
    </div>
    <main class="container">
      <div class="produto-info">
        <img :src="novoItem.img" :alt="novoItem.nome" />
        <h2>{{ novoItem.nome }}</h2>
        <p>{{ novoItem.desc }}</p>
        <p>R$ {{ novoItem.preco.toFixed(2) }}</p>
      </div>
      <div class="dual">
        <button class="add-button" @click="adicionar(index)">Adicionar</button>
        <button class="remove-button" @click="remover(index)">Remover</button>
      </div>
      <button class="ver-mais" @click="mostrar = false">Menos info...</button>
    </main>
  </div>

  <div class="container" v-else-if="mostrarCarrinho">
    <div class="content">
      <div class="produtos">
        <div v-for="(produto, index) of produtos" :key="produto.id" class="produto">
          <div class="produto-info">
            <div>
              <img :src="produto.img" :alt="produto.nome" />
            </div>
            <h2>{{ produto.nome }}</h2>
            <p>{{ produto.descricao }}</p>
            <p>R$ {{ produto.preco.toFixed(2) }}</p>
          </div>
          <div class="dual">
            <button class="add-button" @click="adicionar(index)">Adicionar</button>
            <button class="remove-button" @click="remover(index)">Remover</button>
          </div>
          <button class="ver-mais" @click="verMais(index)">Mais info...</button>
        </div>
      </div>
    </div>
  </div>
  <div class="carrinho" v-else>
    <div class="carrinho-header">
      <h2>Total de itens no carrinho: {{ totalProdutos }}</h2>
    </div>
    <main class="container">
      <div class="carrinho-items">
        <div v-if="carrinho.length == 0">
          <p>Nenhum produto no carrinho.</p>
        </div>
        <div v-else>
          <div v-for="(item, index) of carrinho" :key="item.id" class="carrinho-item">
            <div class="carrinho-item-info">
              <h3>{{ item.nome }}</h3>
              <p>R$ {{ item.preco.toFixed(2) }}</p>
            </div>
                 <div class="dual">
            <button class="add-button" @click="adicionar(item.id)">Adicionar</button>
            <button class="remove-button" @click="remover(index)">Remover</button>
          </div>
          </div>
          <div class="carrinho-total">
            <p>Total: R$ {{ totalPreco.toFixed(2) }}</p>
            <button class="remove-button" @click="limparCarrinho">Limpar carrinho</button>
          </div>
        </div>
      </div>
    </main>
    <button class="btn" @click="mostrarCarrinho = true">Voltar ao inicio</button>
  </div>
</template>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #d6c292e8;
  padding: 10px;
}
main.container {
  background-color: #d6c292e8;
  min-width: 320px;
  min-height: 40vh;
  padding: 2rem;
  box-shadow: 5px 5px 15px rgba(0, 0, 0, 0.2);
  border-radius: 8px;
}
.btn{
  width: 90vh;
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 10px;
  border-radius: 5px;
  cursor: pointer;
  margin: 30px;
  transition: background-color 0.2s, color 0.2s;
}

main h2 {
  font-weight: 600;
  margin-bottom: 2rem;
  position: relative;
}

img {
  max-width: 200px;
  max-height: 280px;
  border-radius: 5px;
}
.btn:hover{
  background-color: #80baf8;
  color: #000000;
  font-weight: bold;
}
.container {
  width: 80%;
  margin: auto;
  padding: 20px;
}
.dual{
  display: grid;
  grid-template-columns: 1fr 1fr;
}
.cart-button {
  background-color: transparent;
  border: 2px solid #fff;
  border-radius: 4px;
  color: #fff;
  font-size: 1rem;
  padding: 0.5rem;
  cursor: pointer;
  margin: 10px;
  transition: background-color 0.2s, color 0.2s;
}

.cart-button:hover {
  background-color: #fff;
  color: #3d2e1e;
}
.content {
  display: flex;
  flex-wrap: wrap;
}

.produtos {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
}

.produto {
  width: calc(33.33% - 20px);
  margin: 10px;
  padding: 20px;
  border-radius: 5px;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  background-color: #d6c292;
}

.produto-info {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.produto-info img {
  width: 100%;
  max-width: 400px;
  height: auto;
  margin-bottom: 1rem;
}

.produto-info h2 {
  font-size: 1.5rem;
  margin-bottom: 0.5rem;
}

.produto-info p {
  margin-bottom: 1rem;
}

.produto-info p:last-of-type {
  font-size: 1.25rem;
  font-weight: bold;
}

.add-button {
  background-color: #007bff;
  color: #fff;
  padding: 10px;
  border-radius: 5px;
  cursor: pointer;
  margin-right: 5px;
}

.remove-button {
  background-color: #dc3545;
  color: #fff;
  padding: 10px;
  margin-left: 5px;
  border-radius: 5px;
  cursor: pointer;
}
.ver-mais {
  background-color: #3d2e1e;
  color: #fff;
  border: none;
  border-radius: 4px;
  font-size: 1rem;
  padding: 0.5rem;
  margin-top: 1rem;
  cursor: pointer;
  transition: background-color 0.2s, color 0.2s;
}

.ver-mais:hover {
  background-color: #fff;
  color: #3d2e1e;
}

.carrinho {
  margin: auto;
  padding: 30px;
}

.carrinho-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin: 30px;
}

.close-button {
  background-color: #007bff;
  color: #fff;
  border: none;
  padding: 10px;
  border-radius: 5px;
  cursor: pointer;
  margin: 30px;
}

.carrinho-items {
  margin-top: 20px;
}

.carrinho-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}

.carrinho-item-info {
  text-align: center;
}

.carrinho-total {
  margin-top: 20px;
  text-align: right;
}

.alert {
  padding: 20px;
  background-color: #f44336;
  color: white;
  width: 50%;
  margin-top: 10px;
  margin-left:290px ;
  border-radius: 10px;
  text-align: center;
}

.alert-sucess {
  padding: 20px;
  background-color: #36f44f;
  color: rgb(255, 255, 255);
  width: 50%;
  margin-top: 10px;
  margin-left:290px ;
  border-radius: 10px;
  text-align: center;
}

.closebtn {
  margin-top: 3px;
  margin-left: 15px;
  color: white;
  font-weight: bold;
  float: right;
  font-size: 22px;
  line-height: 20px;
  cursor: pointer;
  transition: 0.3s;
}

.closebtn:hover {
  color: black;
}
@media (max-width: 480px) {
  main.container {
    min-height: 50vh;
    padding: 1rem;
  }

  main h2 {
    font-size: 1.5rem;
    margin-bottom: 1rem;
  }

  img {
    max-width: 100%;
    max-height: auto;
    margin-bottom: 1rem;
  }

  .container {
    padding: 10px;
  }

  header {
    flex-direction: column;
    align-items: flex-start;
    width: 100vh;
  }

  .cart-button {
    margin-top: 1rem;
  }

  .produtos {
    flex-direction: column;
  }

  .produto {
    width: 100%;
    margin: 10px 0;
    padding: 10px;
  }

  .add-button,
  .ver-mais {
    width: 100%;
  }

  .carrinho {
    padding: 10px;
  }

  .carrinho-header {
    flex-direction: column;
    align-items: flex-start;
  }

  .close-button {
    margin-top: 1rem;
  }

  .carrinho-item {
    flex-direction: column;
    align-items: flex-start;
  }

  .remove-button {
    margin-top: 1rem;
  }

  .carrinho-total {
    margin-top: 1rem;
    text-align: left;
  }
}
</style>
