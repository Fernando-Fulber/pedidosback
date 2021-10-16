<template>
  <div id="app" class="container">
    <div class="form">
      <div class="columns">
        <div class="column">
          <b-field label="Número do pedido">
              <b-input
                v-model="pedido.numero"
                required
              />
          </b-field>
        </div>
        <div class="column is-one-third">
          <b-field label="Data de emissão">
              <b-datepicker
                  v-model="pedido.dataEmissao"
                  icon="calendar-today"
                  locale="pt-BR"
                  trap-focus
                  required
              />
          </b-field>
        </div>
      </div>
      <b-field label="Cliente">
          <b-autocomplete
              v-model="pedido.cliente"
              icon="magnify"
              clearable
              required>
              <template #empty>Nenhum cliente encontrado</template>
          </b-autocomplete>
      </b-field>
      <b-message
          v-if="erros.length > 0"
          title="Erros de validação"
          type="is-danger"
          has-icon
          @close="erros = []">
          <p v-for="(err, idx) in erros" :key="idx">
            {{ err }}
          </p>
      </b-message>
    </div>
    <div class="grid">
      <div class="buttons">
        <b-button @click="adicionarProduto">Adicionar produto</b-button>
        <b-button @click="editarProduto">Editar</b-button>
        <b-button @click="excluirProduto">Excluir</b-button>
      </div>
      <b-table :data="produtosDisplay" :columns="colunasProdutos" />
      <div class="grid-footer">
        <p>Total pedido: {{ totalPedido }}</p>
        <p>Quantidade itens: {{ produtos.length }}</p>
      </div>
      <div class="buttons">
        <b-button @click="salvar">Salvar</b-button>
        <b-button @click="salvarNovo">Salvar/Novo</b-button>
      </div>
    </div>
    <b-modal
        v-model="exibirModalProduto"
        has-modal-card
        trap-focus
        :destroy-on-hide="false"
        aria-role="dialog"
        aria-label="Produto"
        aria-modal>
        <template #default="props">
            <Produto
              :isEditando="isEditandoProduto"
              @salvar="salvarProduto"
              @close="props.close"
            />
        </template>
    </b-modal>
  </div>
</template>

<script>
import Produto from './components/Produto.vue'

export default {
  name: 'App',
  components: {
    Produto,
  },
  data: function () {
    return {
      pedido: {
        numero: null,
        dataEmissao: null,
        cliente: null,
      },
      erros: [],
      exibirModalProduto: false,
      isEditandoProduto: false,
      produtos: [
        { nome: 'Fusca', quantidade: 2, valorUnitario: 15000, valorDesconto: 100 },
        { nome: 'Xeveti', quantidade: 7, valorUnitario: 8000, valorDesconto: 250 },
        { nome: 'Belina', quantidade: 1, valorUnitario: 18000, valorDesconto: 0 },
      ],
      colunasProdutos: [
        { field: 'nome', label: 'Produto' },
        { field: 'quantidade', label: 'Quantidade' },
        { field: 'valorUnitario', label: 'Valor Unitário' },
        { field: 'valorDesconto', label: 'Valor Desconto' },
        { field: 'total', label: 'Total' },
      ],
    }
  },
  computed: {
    produtosComTotal: function () {
      return this.produtos.map(m => ({ ...m,  total: m.quantidade * m.valorUnitario - m.valorDesconto }))
    },
    produtosDisplay: function () {
      //todo: colocar formatação
      return this.produtosComTotal
    },
    totalPedido: function () {
      //tem que ter 1 item pro reduce function
      if (this.produtosComTotal.length < 1) return 0
      
      return this.produtosComTotal.map(m => m.total).reduce((a, b) => a + b)
    }
  },
  methods: {
    salvar() {
      this.erros = []
      
      if (!this.pedido.numero) this.erros.push('Número do pedido é obrigatório')
      if (!this.pedido.dataEmissao) this.erros.push('Data de emissão é obrigatório')
      if (!this.pedido.cliente) this.erros.push('Cliente é obrigatório')

      //retorna true se não tiver erros
      return this.erros.length === 0
    },
    salvarNovo() {
      const valido = this.salvar()
      if (valido) {
        this.pedido = {
          numero: null,
          dataEmissao: null,
          cliente: null,
        }
      }
    },
    adicionarProduto() {
      this.isEditandoProduto = false
      this.exibirModalProduto = true
    },
    editarProduto() {
      this.isEditandoProduto = true
      this.exibirModalProduto = true
    },
    excluirProduto() {
      
    },
    salvarProduto(produto) {
      //todo: implementar
    }
  }
}
</script>

<style>
#app {
  width: 700px;
  margin-top: 64px;
}

.grid {
  margin-top: 32px;
}

.grid-footer {
  margin-top: 16px;
  text-align: right;
}
</style>