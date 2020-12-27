<template>
  <section id="form">
    <form @submit.prevent="enviar">
        <div class="input-group input-group-first">
        <span :class="{sald: saldo, gre: green}">Saldo:<br/><hr/> R$ {{valorTotal}}</span>
        <span class="entradas">Entradas:<br/><hr/> R$ {{valorDeposito}}</span>
        <span class="saidas">Saidas:<br/><hr/> R$ {{valorPagamento}}</span>
      </div>
      <div class="input-group input-group-second">
        <input type="text" placeholder="Nome" v-model="name"/>
        <input type="text" placeholder="R$ " v-model.number="valor">
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
      <label v-if="tipo !== ''">Selecione a categoria</label>
      <select v-if="tipo === 'deposito'" v-model="categoria" class="select">
        <option v-for="(deposito, index) in dep" :key="index" class="option">
          {{deposito}}
        </option>
      </select>
      <select v-if="tipo === 'pagamento'" v-model="categoria" class="select">
        <option v-for="(pagamento, index) in pag" :key="index" class="option">
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
      name: '', valor: null, valorTotal: 0, valorDeposito: 0, valorPagamento: 0,
      tipo: '', categoria: '', selectDeposito: true, selectPagamento: false,
      dep: ["salario", "extra"], pag: ["boleto", "lazer", "educacao", "investimento"],
      transfer: [], descricao: '', saldo: false, green: true, teste: [], valorDepositoTeste: 0,
      valorPagamentoTeste: 0
    }
  },
  methods: {
    limpaForm() {
      this.name = ''
      this.valor = null
      this.descricao = ''
    },
    enviar() {
        if(this.nome === "" || this.valor === null || this.tipo === '' || this.categoria === '') {
          alert("Preenche o valor");
          return;
        } else if(this.valor > this.valorTotal && this.tipo === 'pagamento') {
          alert("Saldo é menor que o valor informado")
          return;
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
        this.getFromLocal()
    },
    deleteTransfer(key) {
      let yesNo = confirm("Excluir?")
      if(yesNo) {
        console.log("deletou" + key)
        let filtro = this.transfer.filter((item) => {
          return (item.hash !== key)
        })        
        this.transfer = filtro;
        this.getFromLocal()
      }
      
      
    },
    getFromLocal() {
      this.valorDeposito=0; this.valorPagamento=0;
        this.transfer.map((item) => {
          if(item.tipo === "deposito") {
            this.valorDeposito += item.valor
          } else if (item.tipo === "pagamento") {
            this.valorPagamento += item.valor
          }
        })
        this.valorTotal = this.valorDeposito - this.valorPagamento
    }
  },
  watch: {
    transfer() {
      localStorage.setItem('myTransfer', JSON.stringify(this.transfer))
      if(this.valorTotal >= 0) {
        this.green = true; this.saldo = false;
      } else {
        this.green = false; this.saldo = true;
      }
    },

  },
  created() {
    const json = localStorage.getItem("myTransfer")
    this.transfer = JSON.parse(json) || [];
    this.getFromLocal()
  }
}
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
  justify-content: center;
  margin: 0 auto;
  max-width: 600px;
}

form input {
    margin-top: 10px;
    width: 250px;
    height: 30px;
}

.input-group {
  display: flex;
}

.input-group-first {
  justify-content: space-around;
  font-size: 25px;
}

.input-group-first span, input, textarea {
  border: 3px solid black;
}

.input-group-first span {
  padding: 10px; color: #fff;
}

.entradas {
  background: green;
}

.saidas {
  background: red;
}

.input-group-second, .input-group-third {
  justify-content: space-around;
}

.input-group-second {
  flex-direction: column;
  align-items: center;
}

.input-group-third {
  margin: 0 auto;
}

.select {
  height: 40px;
  width: 170px;
  margin: 0 auto;
}

textarea {
  height: 70px;
  margin-bottom: 10px;
  margin-top: 10px;
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

.sald {
  background: red;
}

.gre {
  background: green;
}
</style>