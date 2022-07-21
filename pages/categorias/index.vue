<template>
  <v-container>
    <h1>Consulta de Categorias</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col>
           <v-btn  
            elevation="3"
            outlined  
            @click="getCategorias"
            color="primary"
          >
            Pesquisar
            <v-icon 
              style="margin-left:8%">
              mdi-magnify
            </v-icon>
          </v-btn>  
        </v-col>
        <v-col>
          <v-btn
            style="margin-left:-75%"
            elevation="3"
            outlined
            to="/categorias/cadastro"
            color="green"
          >
            Cadastrar
              <v-icon 
              style="margin-left:8%">
              mdi-plus-circle
            </v-icon>
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="categorias"
        :items-per-page="10"
        class="elevation-1"
      >
      <template v-slot:item.actions="{ item }">
        <v-icon
          small
          class="mr-2"
          @click="editItem(item)"
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
  name: 'ConsultaCategoriasPage',
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
        { text: "", value: "actions" }
      ],
      categorias: []
    }
  },
  created () {
    this.getCategorias()
  },
  methods: {
    async getCategorias () {
      this.categorias = await this.$axios.$get('http://localhost:3333/categorias');
    },
    async deletarItem (categoria) {
      try {
        if (confirm(`Deseja deletar a categoria id ${categoria.id} - ${categoria.nome}?`)) {
          let response = await this.$axios.$post('http://localhost:3333/categorias/deletar', { id: categoria.id });
          this.$toast.success(response.message)
          this.getCategorias();
        }
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao atender a requisição. Contate o administrador do sistema.')
      }
    }
  }
}
</script>