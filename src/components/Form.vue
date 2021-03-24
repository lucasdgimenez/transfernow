<template>
  <v-container>
    <v-form 
      @submit.prevent="enviar"
      class="d-flex flex-column justify-center ma-auto"
    >
      <v-row no-gutters>
      <!--<v-row class="input-group input-group-first">
      :class="{sald: saldo, gre: green}"
      valorTotal > 0 ? sald : gre
      <span :class="{sald: saldo, gre: green}">Saldo:<br/><hr/> R$ {{valorTotal}}</span>
-->
        <v-col>
          <v-card 
            :color="valorTotal > 0 ? '#009688' : '#F44'" 
          >
            <v-card-title class="headline">
              Saldo: 
            </v-card-title>
            <v-card-subtitle p-10>
              R$ {{valorTotal}}
            </v-card-subtitle>
          </v-card>
        </v-col>   

        <v-col>
          <v-card color='#009688'>
            <v-card-title class="headline">
              Entradas: 
            </v-card-title>
            <v-card-subtitle p-10>
              R$ {{valorDeposito}}
            </v-card-subtitle>
          </v-card>
        </v-col>

        <v-col>
          <v-card color='#F44'>
            <v-card-title class="headline">
              Saidas: 
            </v-card-title>
            <v-card-subtitle p-10>
              R$ {{valorPagamento}}
            </v-card-subtitle>
          </v-card>
        </v-col>
      </v-row>
      <v-text-field
        v-model="name"
        :counter="10"
        label="Nome"
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

      <label v-if="tipo === 'deposito' || tipo === 'pagamento'">Selecione a categoria</label>
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