<template>
  <v-container>
    <h1>Cadastro de Emprestimo</h1>
    <hr>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col
            cols="2"
          >
            <v-text-field
              v-model="emprestimo.id"
              placeholder="Código"
              label="Código"
              disabled
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col
          cols="12"
          >
          <v-autocomplete
              v-model="emprestimo.idUsuario"
              :items="usuario"
              outlined
              label="Usuário"
              item-text="nome"
              item-value="id"
              :rules="rule"
              required
              :disabled="desabilitar"
            ></v-autocomplete>
          </v-col>
        </v-row>
        <v-row>
            <v-col>
              <v-text-field
                v-model="emprestimo.prazo"
                placeholder="Prazo"
                label="Prazo"
                outlined
                :rules="rule"
                required
                :disabled="desabilitar"
                >
            </v-text-field>
          </v-col>
          <v-col>
              <v-text-field
              v-model="emprestimo.devolucao"
              placeholder="Devolucao"
              label="Devolucao"
              outlined
              disabled
              >
             </v-text-field>

          </v-col>
        </v-row>
        <v-row>
          <v-col
          cols="6"
          >
            <v-autocomplete
                v-model="emprestimo.livros"
                :items="livro"
                outlined
                label="Livro"
                item-text="titulo"
                item-value="id"
                :rules="rule"
                required
                multiple
                chips
                :disabled="desabilitar"
            >
              </v-autocomplete>
          </v-col>
        </v-row>
              <v-btn
          outlined
          to="/emprestimos"
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
  name: 'CadastroEmprestimosPage',
  data () {
    return {
      emprestimo: {
        id: null,
        idUsuario: null,
        prazo: null,
        devolucao: null,
        livros: []
      },
      desabilitar: false,
      valid: false,
      livro: [],
      usuario: [],
      rule: [
      v => !!v || 'Esse campo é obrigatório'
      ]
  }
},
  created () {
    if (this.$route?.params?.id) {
      this.getById(this.$route.params.id);
    }
    this.getUsuarios();
    this.getLivros();
  },
  methods: {
    async persistir () {
      try {
        if (!this.valid) {
          return this.$toast.warning('O formulário de cadastro não é válido!');
        }

        let emprestimo = {
          idUsuario: this.emprestimo.idUsuario,
          prazo: this.emprestimo.prazo,
          devolucao: this.emprestimo.devolucao,
          livros: this.emprestimo.livros
        };

        if (!this.emprestimo.id) {
          await this.$axios.$post('http://localhost:3333/emprestimos', emprestimo);
          this.$toast.success('Cadastro realizado com sucesso!');
          return this.$router.push('/emprestimos');
        }
      } catch (error) {
        console.log(error)
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
      }
    },

    async getUsuarios () {
      this.usuario = await this.$axios.$get('http://localhost:3333/usuarios');
    },

    async getLivros () {
      this.livro = await this.$axios.$get('http://localhost:3333/livros');
    },

    async getById (id) {
      this.emprestimo = await this.$axios.$get(`http://localhost:3333/emprestimos/${id}`);
      console.log(this.emprestimo);
      this.desabilitar = true;
    }

  }
}
</script>