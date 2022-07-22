<template>
  <v-container>
    <h1>Consulta de Usuários</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col>
          <v-btn  
            elevation="3"
            outlined  
            @click="getUsuarios"
            color="primary"
          >
            Pesquisar
            <v-icon 
              style="margin-left:8%">
              mdi-account-search
            </v-icon>
          </v-btn>  
        </v-col>
        <v-col>
          <v-btn
            style="margin-left:-75%"
            elevation="3"
            outlined
            to="/usuarios/cadastro"
            color="green"
          >
            Cadastrar
              <v-icon 
              style="margin-left:8%">
              mdi-account-plus
            </v-icon>
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
      >
      <template v-slot:item.actions="{ item }">
        <v-icon
          small
          class="mr-2"
          @click="editarItem(item)"
        >
          mdi-pencil
        </v-icon>
        <v-icon
          small
          @click="deletarItem(item)"
        >
          mdi-delete
        </v-icon>
      </template>
      </v-data-table>
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
        { text: "", value: "actions" }

      ],
      usuarios: []
    }
  },
  created () {
    this.getUsuarios()
  },
  methods: {
    async getUsuarios () {
      this.usuarios = await this.$axios.$get('http://localhost:3333/usuarios');
    },
    async deletar (usuario) {
      try {
        if (confirm(`Deseja deletar o usuário id ${usuario.id} - ${usuario.nome}?`)) {
          let response = await this.$axios.$post('http://localhost:3333/usuarios/deletar', { id: usuario.id });
          this.$toast.success(response.message)
          this.getUsuarios();
        }
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao atender a requisição. Contate o administrador do sistema.')
      }
    },
    async editarItem (usuario) {
      this.$router.push({
        name: 'usuarios-cadastro',
        params: { id: usuario.id }
        });
    }
  }
} 
</script>