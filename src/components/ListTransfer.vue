<template>
  <div id="listtransfer">
    <h1>Tabela de transferencias</h1>
    <table id="t01" v-if="lista.length > 0">
        <tr>
          <th>Nome</th>
          <th>Valor</th> 
          <th>Tipo</th>
          <th>Categoria</th>
          <th>Descrição</th>
          <th>Hash</th>
        </tr>
        <tr v-for="(item, index) in lista" :key="index">
          <td>{{item.name}}</td>
          <td>{{item.valor}}</td>
          <td>{{item.tipo}}</td>
          <td>{{item.categoria}}</td>
          <td>{{item.descricao}}</td>
          <td>{{item.hash}}</td>
          <td><button class="excluir" @click="deleteItem(item.hash)">X</button></td>
        </tr>
    </table>
  </div>
</template>

<script>
export default {
  name: 'ListTransfer',
  props: ['lista', 'delete'],
  methods: {
    deleteItem(hash) {
      this.delete(hash)
    }
  }
}
</script>

<style scoped>
table {
  width:100%;
}
table, th, td {
  border: 1px solid black;
  border-collapse: collapse;
}
th, td {
  padding: 15px;
  text-align: left;
}
#t01 tr:nth-child(even) {
  background-color: #eee;
}
#t01 tr:nth-child(odd) {
 background-color: #fff;
}
#t01 th {
  background-color: black;
  color: white;
}
.excluir {
  background: red;
  color: #fff;
  border: none;
}
</style>