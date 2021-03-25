<template>
  <v-container>
    <v-form 
      @submit.prevent="enviar"
      class="d-flex flex-column justify-center ma-auto"
    >
      <h1 class="text-center">Money control</h1>
      <v-row no-gutters class="status">
        <v-col>
          <v-card 
            :color="valorTotal >= 0 ? '#009688' : '#F44'"
            class="white--text"
          >
            <v-card-title class="headline">
              Saldo: 
            </v-card-title>
            <v-card-text class="white--text valor">
              R$ {{valorTotal}}
            </v-card-text>
          </v-card>
        </v-col>   

        <v-col>
          <v-card color='#009688' class="white--text">
            <v-card-title class="headline">
              Entradas: 
            </v-card-title>
            <v-card-text class="white--text valor">
              R$ {{valorDeposito}}
            </v-card-text>
          </v-card>
        </v-col>

        <v-col>
          <v-card color='#F44' class="white--text">
            <v-card-title class="headline">
              Saidas: 
            </v-card-title>
            <v-card-text class="white--text valor">
              R$ {{valorPagamento}}
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>

      <v-text-field
        v-model="nome"
        :counter="10"
        label="Nome"
        class="mt-4"
        required
      >
      </v-text-field>
      <v-text-field
        v-model.number="valor"
        :counter="6"
        label="R$"
        required
      >
      </v-text-field>
      
      <v-row class="justify-center">
        <v-checkbox
          v-model="tipo"
          label="Deposito"
          color="success"
          value="deposito"
        >
        </v-checkbox>
        <v-checkbox
          v-model="tipo"
          label="Pagamento"
          color="red"
          value="pagamento"
        >
        </v-checkbox>
      </v-row>

      <label 
        v-if="tipo === 'deposito' || tipo === 'pagamento'"
        class="text-center"
      >
        Selecione a categoria
      </label>
      <v-select 
        v-if="tipo === 'deposito'"
        v-model="categoria"
        :items="dep"
        class="select"
        label="deposito"
      >
      </v-select>
      <v-select 
        v-if="tipo === 'pagamento'" 
        v-model="categoria"
        :items="pag"
        class="select"
        label="pagamento"
      >
      </v-select>

      <v-textarea
        name="input-7-1"
        label="Descricao"
        v-model="descricao"
      ></v-textarea>

      <v-btn
        type="submit"
        color="primary"
      >
        Enviar
      </v-btn>
    </v-form>
    
    <hr/>

    <ListTransfer :lista="transfer" :delete="deleteTransfer"/>

  </v-container>
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
      nome: '', valor: 0, valorTotal: 0, valorDeposito: 0, valorPagamento: 0,
      tipo: '', categoria: '', selectDeposito: true, selectPagamento: false,
      dep: ["salario", "extra"], pag: ["boleto", "lazer", "educacao", "investimento"],
      transfer: [], descricao: '', saldo: false, green: true, teste: [], valorDepositoTeste: 0,
      valorPagamentoTeste: 0
    }
  },
  methods: {
    limpaForm() {
      this.nome = ''
      this.valor = 0
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
        nome: this.nome,
        valor: parseFloat(this.valor),
        tipo: this.tipo,
        categoria: this.categoria,
        descricao: this.descricao,
        hash: Math.round(Math.random() * Date.now())
      })
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
    tipo() {
      console.log(this.tipo)
    }
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
  max-width: 800px;
}

.valor {
  font-size: 2rem;
}

@media only screen and (max-width: 545px) {
  .status {
    flex-direction: column;
  }
  .status div {
    margin: 2px;
    align-items: center;
  }
  .card {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .titulo_valor {
    flex: 2;
    background: chocolate;
  }
  .valor {
    font-size: 1.5rem;
    flex: 2;
  }
}
</style>