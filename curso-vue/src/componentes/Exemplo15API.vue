<!--Script-->
<script setup>

// Importar o bootstrap
import 'bootstrap/dist/css/bootstrap.min.css';

// Importação - VUE
import { onMounted, ref } from 'vue';

// Vetor de produtos
let produtos = ref([]);

// OnMounted
onMounted(() => {
   fetch('http://localhost:3000/produtos')
   .then(requisicao => requisicao.json())
   .then(retorno => produtos.value = retorno);
});

// Objeto do tipo produto
let obj = ref({ id: 0, produto: '', valor: 0 });

// Função para cadastrar produtos
function cadastrar(event){
    // Alerta
    alert('Teste');

    // Requisição
    fetch('http://localhost:3000/produtos', {
        method: 'POST',
        body: JSON.stringify(obj.value),
        headers: {'Content-Type': 'application/json'}
    })
    .then(requisicao => requisicao.json())
    .then(retorno => {

        //cadastrar o produto no vetor
        produtos.value.push(retorno)

        //Limpar inputs
        obj.value.produto = '';
        obj.value.valor = 0;
    });

    // PreventDefault
    event.preventDefault();
}

</script>

<!--CSS-->
<style>
   
   form {
    width: 50%;
    margin: 30px auto;
    text-align: center;
   }

   input {
    margin-bottom: 10px;
   }

</style>
   
<!--HTML-->
<template>

<!--Formulário-->
  <form @submit="cadastrar">
    <p>{{obj}}</p>
     <input type="text" placeholder="produto" class="form-control" v-model="obj.produto">    
     <input type="number" placeholder="valor" class="form-control" v-model="obj.valor">   
     <input type="submit" value="Cadastrar" class="btn btn-primary">    
  </form>

  <table class="table table-striped">
    <thead>
        <tr>
            <th>Produto</th>
            <th>Valor</th>
            <th>Selecionar</th>
        </tr>
    </thead>
    <tbody>
        <tr v-for="p in produtos" :key="p.id">
            <td>{{p.produto}}</td>
            <td>{{p.valor}}</td>
            <td><button class="btn btn-primary">Selecionar</button></td>
        </tr>
    </tbody>
  </table>
</template>
