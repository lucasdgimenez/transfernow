<template>
  <section id="form">
    <form @submit.prevent="enviar">
        <div class="input-group input-group-first">
        <span>Valor total: R$ {{valorTotal}}</span>
        <span>Valor deposito: R$ {{valorDeposito}}</span>
        <span>Valor pagamento: R$ {{valorPagamento}}</span>
      </div>
      <div class="input-group input-group-second">
        <input type="text" placeholder="Nome" v-model="name"/>
        <input type="text" placeholder="R$ " v-model="valor">
      </div>
      <div class="input-group input-group-third">
        <label nameFor="deposito">
          <input type="radio" name="deposito" value="deposito" v-model="tipo">
          Deposito
        </label>
        <label nameFor="pagamento">
          <input type="radio" name="pagamento" value="pagamento" v-model="tipo">
          Pagamento
        </label>
      </div>      
      <label>Selecione a categoria</label>
      <select v-if="tipo === 'deposito'" v-model="categoria">
        <option v-for="(deposito, index) in dep" :key="index">
          {{deposito}}
        </option>
      </select>
      <select v-if="tipo === 'pagamento'" v-model="categoria">
        <option v-for="(pagamento, index) in pag" :key="index">
          {{pagamento}}
        </option>
      </select>
      <textarea type="text" placeholder="Descrição: " v-model="descricao"/>
      <button type="submit" class="btn">Enviar</button>
    </form>
    
    <hr/>

    <ListTransfer :lista="transfer" :delete="deleteTransfer"/>

  </section>
</template>

<script>
import ListTransfer from "./ListTransfer"

export default {
  name: "Form",
  components: {
    ListTransfer
  },
  data() {
    return {
      name: '',
      valor: 0,
      valorTotal: 0,
      valorDeposito: 0,
      valorPagamento: 0,
      tipo: '',
      categoria: '',
      selectDeposito: true,
      selectPagamento: false,
      dep: ["salario", "grana extra", "sorteio"],
      pag: ["boleto", "lazer", "educacao", "investimento"],
      transfer: [],
      descricao: ''
    }
  },
  methods: {
    limpaForm() {
      this.name = ''
      this.valor = 0
      this.descricao = ''
    },
    enviar() {
        if(this.tipo === "deposito") {
          this.valorDeposito = parseFloat(this.valorDeposito) + parseFloat(this.valor)
          this.valorTotal = parseFloat(this.valorTotal) + parseFloat(this.valor)
        } else if(this.tipo === "pagamento") {
          this.valorPagamento = parseFloat(this.valorPagamento) + parseFloat(this.valor)
          this.valorTotal = parseFloat(this.valorTotal) - parseFloat(this.valor)
        }
        this.transfer.push({
          name: this.name,
          valor: parseFloat(this.valor),
          tipo: this.tipo,
          categoria: this.categoria,
          descricao: this.descricao,
          hash: Math.round(Math.random() * Date.now())
        })
        alert("Transferencia efetuada com sucesso")
        this.limpaForm()
        console.log(this.transfer)
    },
    deleteTransfer(key) {
      let yesNo = confirm("Excluir?")
      if(yesNo) {
        console.log("deletou" + key)
        let filtro = this.transfer.filter((item) => {
          return (item.hash !== key)
        })
        return this.transfer = filtro
      }
    }
  },
  watch: {
    transfer() {
      localStorage.setItem('myTransfer', JSON.stringify(this.transfer))

    }
  },
  created() {
    const json = localStorage.getItem("myTransfer")
    this.transfer = JSON.parse(json) || []
  }
}
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
  justify-content: center;
    margin: 0 auto;

  width: 700px;
}

form input {
    margin-top: 10px;
}

.input-group {
  display: flex;
}

.input-group-first {
  justify-content: space-around;
}

.input-group-second, .input-group-third {
  justify-content: space-between;
}

.btn {
  background: chocolate;
  color: #fff;
  width: 200px;
  border: 0;
  margin: 0 auto;
  padding: 10px;
  font-size: 20px;
  cursor: pointer;
  border-radius: 5px;
}

.btn:hover {
  background: crimson;
}


</style>