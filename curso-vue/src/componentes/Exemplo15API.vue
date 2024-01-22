<!-- Script -->
<script setup>

// Importar o bootstrap
import 'bootstrap/dist/css/bootstrap.min.css';

// Importar - VUE
import { onMounted, ref } from 'vue';

// Vetor de produtos
let produtos = ref([]);

// Visibilidade dos botões
let btnCadastrar = ref(true);

// Objeto do tipo produto
let obj = ref({ id: 0, produto: '', valor: 0 });


//-----------------------------------------------------------------------------------------------------------------------------------------------------
//                                  CRUD 
// Função para cadastrar produtos
function cadastrar(event){
    // Alerta de teste
    alert('Teste');

    // Requisição para cadastrar o produto
    fetch('http://localhost:3000/produtos', {
        method: 'POST',
        body: JSON.stringify(obj.value),
        headers: {'Content-Type': 'application/json'}
    })
    .then(requisicao => requisicao.json())
    .then(retorno => {

        // Adicionar o produto ao vetor
        produtos.value.push(retorno)

        // Limpar inputs
        obj.value.produto = '';
        obj.value.valor = 0;
    });

    // Evitar o comportamento padrão do formulário
    event.preventDefault();
}

// Função para selecionar um produto específico
function selecionar(indice){
    // Atualizar o objeto para o produto selecionado
    obj.value ={
        id : produtos.value[indice].id,
        produto : produtos.value[indice].produto,
        valor : produtos.value[indice].valor
    }

    // Mudar a visibilidade dos botões
    btnCadastrar.value = false;
}

// Função para editar produtos
function editar() {

    // Requisição para editar o produto
    fetch('http://localhost:3000/produtos/' + obj.value.id, {
        method: 'PUT',
        body: JSON.stringify(obj.value),
        headers: {'Content-Type': 'application/json'}
    })
    .then(requisicao => requisicao.json())
    .then(retorno => {

        // Obter o índice do vetor
        let indiceProduto = produtos.value.findIndex(objP => objP.id === retorno.id);

        // Editar o produto no vetor
        produtos.value[indiceProduto] = retorno;

        // Limpar inputs
        obj.value.id = 0;
        obj.value.produto = '';
        obj.value.valor = 0;

        // Mudar a visibilidade dos botões
        btnCadastrar.value = true;
    });
}

// Função para remover produtos
function remover() {

// Requisição para editar o produto
fetch('http://localhost:3000/produtos/' + obj.value.id, {
    method: 'DELETE',
    headers: {'Content-Type': 'application/json'}
})
.then(requisicao => requisicao.json())
.then(retorno => {

    // Obter o índice do vetor
    let indiceProduto = produtos.value.findIndex(objP => obj.value.id);

    // Editar o produto no vetor
    produtos.value.splice[indiceProduto,1];

    // Limpar inputs
    obj.value.id = 0;
    obj.value.produto = '';
    obj.value.valor = 0;

    // Mudar a visibilidade dos botões
    btnCadastrar.value = true;
});

}

// Quando o componente é montado
onMounted(() => {
   // Buscar produtos da API
   fetch('http://localhost:3000/produtos')
   .then(requisicao => requisicao.json())
   .then(retorno => produtos.value = retorno);
});

//-----------------------------------------------------------------------------------------------------------------------------------------------------


</script>

<!-- Estilos CSS -->
<style>
   
   form {
    width: 50%;
    margin: 30px auto;
    text-align: center;
   }

   input {
    margin-bottom: 10px;
   }

   .espacamentoBtn {
     margin-left: 5px;
     margin-right: 5px;
   }

</style>
   
<!-- HTML -->
<template>

<!-- Formulário -->
  <form @submit="cadastrar">
    <p>{{ obj }}</p>
    <input type="hidden" v-model="obj.id">
    <input type="text" placeholder="produto" class="form-control" v-model="obj.produto">    
    <input type="number" placeholder="valor" class="form-control" v-model="obj.valor"> 

    <!-- Botões de Cadastro, Edição e Remoção -->
    <input type="submit" v-if="btnCadastrar" value="Cadastrar" class="btn btn-primary">    
    <input type="button" @click="editar" v-if="!btnCadastrar" value="Editar" class="btn btn-primary espacamentoBtn">
    <input type="button" @click="remover" v-if="!btnCadastrar" value="Remover" class="btn btn-primary espacamentoBtn" >
  </form>

  <!-- Tabela de Produtos -->
  <table class="table table-striped">
    <thead>
        <tr>
            <th>Produto</th>
            <th>Valor</th>
            <th>Selecionar</th>
        </tr>
    </thead>
    <tbody>
        <!-- Loop para exibir produtos -->
        <tr v-for="(p,indice) in produtos" :key="p.id">
            <td>{{ p.produto }}</td>
            <td>{{ p.valor }}</td>
            <!-- Botão Selecionar -->
            <td><button @click="selecionar(indice)" class="btn btn-primary">Selecionar</button></td>
        </tr>
    </tbody>
  </table>
</template>
