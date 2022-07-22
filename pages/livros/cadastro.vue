<template>
  <v-container>
    <h1>Cadastro de Livros</h1>
    <hr>
    <v-form v-model="valid">
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
            <v-text-field
              v-model="livro.titulo"
              placeholder="Titulo"
              label="Titulo"
              outlined
              :rules="rule"
              required
            >
            </v-text-field>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-textarea
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
            <v-autocomplete
              v-model="livro.idAutor"
              :items="autores"
              outlined
              label="Autor"
              item-text="nome"
              item-value="id"
              :rules="rule"
              required
            ></v-autocomplete>
          </v-col>
          <v-col
          cols="2"
          >
          <v-autocomplete
              v-model="livro.idCategoria"
              :items="categorias"
              outlined
              label="Categoria"
              item-text="nome"
              item-value="id"
              :rules="rule"
              required
            ></v-autocomplete>
          </v-col>
        </v-row>
              <v-btn
          outlined
          to="/livros"
        >
          Cancelar
        </v-btn>
        <v-btn
          outlined
          @click="persistir"
        >
          Salvar
        </v-btn>
      </v-container>
    </v-form>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroLivrosPage',
  data () {
    return {
      valid: false,
      livro: {
        id: null,
        titulo: null,
        sinopse: null,
        idCategoria: null,
        idAutor: null
      },
      categorias: [],
      autores: [],
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ]
    }
  },
  created () {
    if (this.$route?.params?.id) {
    this.getById(this.$route.params.id)
    }
    this.getAutores();
    this.getCategorias();
  },
  methods: {
    async persistir () {
      try {
        if (!this.valid) {
        return this.$toast.warning('O formulário de cadastro não é válido!');
        }
        let livro = {
          titulo: this.livro.titulo,
          sinopse: this.livro.sinopse,
          idCategoria: this.livro.idCategoria,
          idAutor: this.livro.idAutor
      };
              //caso não tenha ID na tela, significa que é um cadastro NOVO
        //por isso ele vai apenas com o objeto da categoria para o cadastro
        //como no final tem um RETURN, ele vai cair fora da função PERSISTIR
      if (!this.livro.id) {
        await this.$axios.$post('http://localhost:3333/livros', livro);
        this.$toast.success('Cadastro realizado com sucesso!');
        return this.$router.push('/livros');
      }

        await this.$axios.$post(`http://localhost:3333/livros/${this.livro.id}`, livro);
        this.$toast.success('Cadastro atualizado com sucesso!');
        return this.$router.push('/livros');
        
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
      }
    },
    async getAutores () {
      this.autores = await this.$axios.$get('http://localhost:3333/autores');
    },
    async getCategorias () {
      this.categorias = await this.$axios.$get('http://localhost:3333/categorias');
    },
    async getById (id) {
      this.livro = await this.$axios.$get(`http://localhost:3333/livros/${id}`);
    }
  }
}
</script>