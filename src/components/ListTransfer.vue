<template>
  <v-col class="table d-flex flex-wrap justify-space-around ma-auto">
    <v-card 
      v-for="item in lista" :key="item.hash" 
      class="d-flex white--text mt-20 registro mb-4"
      :color="item.tipo === 'deposito' ? '#009688' : '#F44'"
    >
      <article>
        <label>Nome: {{item.nome}} </label>
        <label>Valor: {{item.valor}} </label>
        <label>Tipo: {{item.tipo}}</label>
        <label>Categoria: {{item.categoria}} </label>
        <label>Descrição: {{item.descricao}} </label>
      </article>
      <v-btn class="error" @click="deleteItem(item.hash)">X</v-btn>
    </v-card>
  </v-col>
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
.table {
  max-width: 800px;
  font-size: 1.5rem;
}

.registro {
  border: 3px solid #000;
}

article {
  display: flex;
  flex-direction: column;
  width: 300px;
}

.excluir {
  height: 1rem;
  width: 1rem;
  color: white;
}
</style>