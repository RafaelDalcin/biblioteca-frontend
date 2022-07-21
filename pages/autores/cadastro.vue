<template>
  <v-container>
    <h1>Cadastro de Autores</h1>
    <hr>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col
            cols="2"
          >
            <v-text-field
              v-model="autor.id"
              placeholder="Código"
              label="Código"
              disabled
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col
          cols="6"
          >
            <v-text-field
              v-model="autor.nome"
              placeholder="Nome"
              label="Nome"
              :rules="rule"
              outlined
              required
            />
          </v-col>
          <v-col
          cols="6"
          >
            <v-text-field
              v-model="autor.email"
              placeholder="E-mail"
              label="E-mail"
              :rules="rule"
              outlined
              required
            />
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      outlined
      to="/autores"
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
  name: 'CadastroAutoresPage',
  data () {
    return {
      valid: false,
      autor: {
        id: null,
        nome: null,
        email: null,
      },
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ]
    }
  },
  methods: {
    async cadastrar () {
      try {
        if (!this.valid) {
        return this.$toast.warning('O formulário de cadastro não é válido!');
        }
        let autor = {
          nome: this.autor.nome,
          email: this.autor.email,
        };
        await this.$axios.$post('http://localhost:3333/autores', autor);
        this.$toast.success('Cadastro realizado com sucesso');
        this.$router.push('/autores');

      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
      }
    }
  }
}
</script>