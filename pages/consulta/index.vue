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
      <v-container v-if="!disponivel">
        <v-card
          elevation="2"
          outlined
          shaped
        >
          <v-card-title>
            LIVRO: {{livroSelecionado.titulo}}
          </v-card-title>
          <v-card-text>
            NÃO SE ENCONTRA DISPONÍVEL PARA EMPRÉSTIMO !!
            DADOS DO EMPRÉSTIMO SÃO ID: {{consultas.emprestimo_livros?.id}}
            Devolução: {{consultas.devolucao}}
          </v-card-text>
        </v-card>
      </v-container>
      <v-container v-else> 
        <v-card
          elevation="2"
          outlined
          shaped  
        >
          <v-card-title>
            LIVRO: {{livroSelecionado.titulo}}
          </v-card-title>
          <v-card-text>
            SE ENCONTRA DISPONÍVEL PARA EMPRÉSTIMO !!
          </v-card-text>
        </v-card>
      </v-container>
    </v-container>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroLivrosPage',
  data () {
    return {
      livroSelecionado: {},
      livro: {
        id: null,
      },  
      livros: [],
      consultas: [],
      disponivel: null
    }
  },

  created () {
    this.getLivros();
  },

  methods: {
    async getLivros () {
      this.livros = await this.$axios.$get('http://localhost:3333/livros');
      console.log(this.livros);
    },
    async consultar () {
      this.consultas = await this.$axios.$post('http://localhost:3333/emprestimos-dados', {idLivro: this.livro.id})
      console.log(this.consultas);
      this.livros.forEach((livro) => {
        if (this.livro.id === livro.id) {
          this.livroSelecionado = livro;
        }
      })

      if (!this.consultas.id) {
        // this.consultas.devolucao = 'EM ABERTO'
        return this.disponivel = true;
      } 
      else {
        return this.disponivel = false;
      }
    },
  }
}
</script>

