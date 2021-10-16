<template>
  <div class="modal-card">
    <header class="modal-card-head">
      <p class="modal-card-title">{{ isEditando ? 'Editar' : 'Novo' }} produto</p>
      <button
        type="button"
        class="delete"
        @click="$emit('close')"
      />
    </header>
    <section class="modal-card-body">
      <div class="container">
        <div class="form">
            <div class="columns">
        <div class="column">
          <b-field label="Nome">
              <b-input
                required
              />
          </b-field>
          <b-field label="Quantidade">
              <b-input
                required
              />
          </b-field>
          <b-field label="Valor Unitário">
              <b-input
                required
              />
          </b-field>
          <b-field label="Valor Desconto">
              <b-input
                required
              />
          </b-field>
        </div>
      </div>
        </div>
      </div>
    </section>
    <footer class="modal-card-foot">
      <b-button
        label="Salvar"
        type="is-primary"
        @click="salvar"
      />
    </footer>
  </div>
</template>

<script>
export default {
  props: {
    isEditando: Boolean,
  },
  data: function () {
    return {
      produto: {
        //nome
        //quantidade
        //valor unitario
        //valor desconto
      }
    }
  },
  methods: {
    validar() {
      //todo: implementar validacao
    },
    salvar() {
      const valido = this.validar()
      if (valido) {
        this.$emit('salvar', this.produto)
      }
    }
  },
}
</script>