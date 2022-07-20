<template>
  <v-container>
    <h1>Cadastro de Usuários</h1>
    <hr>
    <v-form>
      <v-container>
        <v-row>
          <v-col
            cols="2"
          >
            <v-text-field
              v-model="livro.id"
              placeholder="Código"
              label="Código"
              disabled
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-textarea
              v-model="livro.titulo"
              placeholder="Titulo"
              label="Titulo"
              outlined
            >
            </v-textarea>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="livro.sinopse"
              placeholder="Sinopse"
              label="Sinopse"
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col
          cols="2"
          >
            <v-text-field
              v-model="livro.idCategoria"
              placeholder="Categoria"
              label="Categoria"
              outlined
            />
          </v-col>
          <v-col
          cols="2"
          >
            <v-text-field
              v-model="livro.idAutor"
              placeholder="Autor"
              label="Autor"
              outlined
            />
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      outlined
      to="/livros"
    >
      Cancelar
    </v-btn>
    <v-btn
      outlined
      @click="cadastrar"
    >
      Cadastrar
    </v-btn>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroLivrosPage',
  data () {
    return {
      livro: {
        id: null,
        titulo: null,
        sinopse: null,
        idCategoria: null,
        idAutor: null,
      }
    }
  },
  methods: {
    async cadastrar () {
      let livro = {
        titulo: this.livro.titulo,
        sinopse: this.livro.sinopse,
        idCategoria: this.livro.idCategoria,
        idAutor: this.livro.idAutor
      };
      let response = await this.$axios.$post('http://localhost:3333/livros', livro);
      console.log(response);
    }
  }
}
</script>