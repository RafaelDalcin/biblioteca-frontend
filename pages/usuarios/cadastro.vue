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
              v-model="usuario.id"
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
              v-model="usuario.nome"
              placeholder="Nome"
              label="Nome"
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.cpfcnpj"
              placeholder="CPF/CNPJ"
              label="CPF/CNPJ"
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.email"
              placeholder="E-mail"
              label="E-mail"
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.telefone"
              placeholder="Telefone"
              label="Telefone"
              outlined
            />
          </v-col>
        </v-row>
            <v-btn
      outlined
      to="/usuarios"
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
    </v-form>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroUsuariosPage',
  data () {
    return {
      usuario: {
        id: null,
        nome: null,
        cpfcnpj: null,
        email: null,
        telefone: null,
      }
    }
  },
  methods: {
    async cadastrar () {
      let usuario = {
        nome: this.usuario.nome,
        cpfcnpj: this.usuario.cpfcnpj,
        email: this.usuario.email,
        telefone: this.usuario.telefone
      };
      let response = await this.$axios.$post('http://localhost:3333/usuarios', usuario);
      console.log(response);
    }
  }
}
</script>