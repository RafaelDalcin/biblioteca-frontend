<template>
  <v-container>
    <h1>Consulta de Livros Emprestados</h1>
    <hr>
    <v-form>
      <v-container>
          <v-autocomplete
          outlined
          v-model="livro.id"
          :items="livros"
          label="Livro"
          item-text="titulo"
          item-value="id"

          required
          >
          </v-autocomplete>
      </v-container>
    </v-form>
    <v-container>
      <v-btn
      outlined
      @click="consultar"
      >
      Consultar
      </v-btn>
    </v-container>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroLivrosPage',
  data () {
    return {
      livro: {
        id: null,
      },  
      livros: [],
      consultas: []
    }
  },

  created () {
    this.getLivros()
  },

  methods: {
    async getLivros () {
      this.livros = await this.$axios.$get('http://localhost:3333/livros');
    },
    async consultar () {
      this.consultas = await this.$axios.$post('http://localhost:3333/emprestimos-dados', {idLivro: this.livro.id})
      console.log(this.consultas);
    }
  }
}
</script>

