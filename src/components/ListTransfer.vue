<template>
    <table id="t01" v-if="lista.length > 0">
        <tr>
          <th>Nome</th>
          <th>Valor</th> 
          <th>Tipo</th>
          <th>Categoria</th>
          <th>Descrição</th>
          <th>Hash</th>
          <th>Excluir</th>
        </tr>
        <tr v-for="(item, index) in lista" :key="index" class="linha" :class="{entry: item.tipo === 'deposito' ? true : false}">
          <td>{{item.name}}</td>
          <td>{{item.valor}}</td>
          <td >{{item.tipo}}</td>
          <td>{{item.categoria}}</td>
          <td>{{item.descricao}}</td>
          <td>{{item.hash}}</td>
          <td><button class="excluir" @click="deleteItem(item.hash)">X</button></td>
        </tr>
    </table>
</template>

<script>
export default {
  name: 'ListTransfer',
  props: ['lista', 'delete'],
  entrada: false,
  methods: {
    deleteItem(hash) {
      this.delete(hash)
    }
  }
}
</script>

<style scoped>
table {
  width:1000px; margin: 0 auto;
}
table, th, td {
  border: 1px solid black;
  border-collapse: collapse;
}
th, td {
  padding: 15px;
  text-align: left;
  font-weight: bold; font-size: 20px;
}
#t01 th {
  background-color: black;
  color: white;
}
.linha {
    color: white;
    background: #d4707a;
}
.entry {
  background: #40c26e;
}


.excluir {
  background: red;
  border-radius: 50%;
  color: white;
  border: none;
}
</style>