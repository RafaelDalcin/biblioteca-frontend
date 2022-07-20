<template>
  <v-container>
    <h1>Consulta de Usuários</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col>
          <v-btn
            outlined
            @click="getUsuarios"
          >
            Pesquisar
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            outlined
            to="/usuarios/cadastro"
          >
            Cadastrar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="usuarios"
        :items-per-page="10"
        class="elevation-1"
      ></v-data-table>
    </v-container>
  </v-container>
</template>

<script>
export default {
  name: 'ConsultaUsuariosPage',
  data () {
    return {
      headers: [
        {
          text: 'Código', //nome da coluna
          align: 'center', //alinhamento -center, end, start
          sortable: false, //se permite ordenação dos dados por essa coluna
          value: 'id', //é o dado que essa coluna vai receber
        },
        {
          text: 'Nome',
          align: 'center',
          sortable: false,
          value: 'nome',
        },
        {
          text: 'CPF/CNPJ', //nome da coluna
          align: 'center', //alinhamento -center, end, start
          sortable: false, //se permite ordenação dos dados por essa coluna
          value: 'cpfcnpj', //é o dado que essa coluna vai receber
        },
        {
          text: 'E-mail', //nome da coluna
          align: 'center', //alinhamento -center, end, start
          sortable: false, //se permite ordenação dos dados por essa coluna
          value: 'email', //é o dado que essa coluna vai receber
        },
        {
          text: 'Telefone', //nome da coluna
          align: 'center', //alinhamento -center, end, start
          sortable: false, //se permite ordenação dos dados por essa coluna
          value: 'telefone', //é o dado que essa coluna vai receber
        },

      ],
      usuarios: []
    }
  },
  created () {
    this.getUsuarios()
  },
  methods: {
    async getUsuarios () {
      this.usuarios = await this.$axios.$get('http://localhost:3333/usuarios/');
    }
  }
}
</script>