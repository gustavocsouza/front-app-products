<template>
  <div class="main">
    <div class="input-container">
      <input v-model="name" type="text" name="name" placeholder="Nome" >
      <input v-model="description" type="text" name="description" placeholder="Descrição">
      <input v-model="price" type="text" name="price" placeholder="Preço">
      <button @click="adicionarProduto">Adicionar Produto</button>
    </div>
    <div class="listagem-produtos">
      <div class="card-produto" v-for="produto in listaDeProdutos" v-bind:key="produto.id">
        <img src="https://picsum.photos/300/200" alt="">
        <h2 class="titulo-produto">{{ produto.name }}</h2>
        <p class="descricao-produto">{{ produto.description }}</p>
        <p style="fontWeight: bold" class="descricao-produto">R${{ produto.price.replace('.',',') }}</p>
        <button @click="deletarProduto(produto.id)">Excluir</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
axios.defaults.baseURL = 'http://localhost:8000/api/'
export default {
  data() {
    return {
      listaDeProdutos : [],
      name: '',
      description: '',
      price: '',
    };
  },
   methods: {
    async adicionarProduto() {
      var data = {name:this.name, description: this.description, price: this.price.replace(',', '.')}
      await axios.post('products', data).then((response) => {
        console.log(response)
        this.reloadPage();
      })
      this.name = '';
      this.description = '';
      this.price = '';
    },
    async listarProdutos(){
      await axios.get('products').then((response)=>{
        console.log(response)
        this.listaDeProdutos = response.data.products;
      })
    },
    async deletarProduto(id){
      await axios.delete(`products/${id}`).then((response)=>{
        console.log(response);
        this.reloadPage();
      })
    },
    reloadPage(){
      window.location.reload();
    }
  },
  
  created() {
    this.listarProdutos();
  }
}
</script>

<style>
.main {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 50px;
  font-family: 'Poppins', sans-serif;
}

.input-container {
  display: flex;
  flex-direction: column;
  max-width: 400px;
  width: 100%;
  
}
.input-container input{
  padding: 12px;
  border: none;
  border-radius: 20px;
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
  margin-bottom: 10px;
}
.input-container button{
  padding: 10px;
  border: none;
  background-color: rgb(193, 193, 250);
  color: rgb(22, 22, 22);
  border-radius: 20px;
  box-shadow: rgba(0, 0, 0, 0.24) 0px 3px 8px;
  font-weight: bold;
}

.listagem-produtos {
  display: flex;
  flex-wrap: wrap;  
  justify-content: space-between;
  max-width: 1000px;
  width: 100%;
}

.card-produto {
  display: flex;
  flex-direction: column;
  align-items: center; 
  justify-content: space-between;  
  box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
  width: 100%;
  max-width: 300px;
  height: 420px;
  margin-top: 30px;
  border-radius: 20px;
}

.card-produto h2 {
  padding: 10px;
  margin: 0;
}
.card-produto p {
  padding: 10px;
  margin: 0;
}

.card-produto button {
  width: 100%;
  padding: 10px;  
  border: none;
  color: rgb(19, 19, 19);
  background-color: rgb(219, 164, 164);
  font-weight: bold;
  border-radius: 20px;
}

.card-produto button:hover{
  background-color: rgb(211, 105, 105);
  cursor: pointer;
}
.card-produto img {
  border-radius: 10px 10px 0px 0;
}

@media only screen and (max-width: 600px ) {
  .listagem-produtos {
    justify-content: center;
  }
}

</style>