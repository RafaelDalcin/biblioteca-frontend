<template>
  <v-container>
    <h1>Consulta de Emprestimos</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col>
           <v-btn  
            elevation="3"
            outlined  
            @click="getEmprestimos"
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
            to="/emprestimos/cadastro"
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
        :items="emprestimos"
        :items-per-page="10"
        class="elevation-1"
      >
      <template v-slot:item.actions="{ item }">
          <v-icon
            color="blue"
            medium
            class="mr-2"
            @click="editarItem(item)"
            >
            mdi-pencil
          </v-icon>
          <v-icon
            color="red"
            medium
            @click="deletarItem(item)"
            >
            mdi-delete
          </v-icon>
          <v-icon
            color="red"
            medium
            @click="encerrarEmprestimo(item)"
            >
            mdi-sword
          </v-icon>
        </template>
      </v-data-table>
    </v-container>
  </v-container>
</template>

<script>
export default {
  name: 'ConsultaEmprestimosPage',
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
          text: 'Prazo',
          align: 'center',
          sortable: false,
          value: 'prazo',
        },
        {
          text: 'Devolução',
          align: 'center',
          sortable: false,
          value: 'devolucao',
        },
        {
          text: 'Data do Empréstimo',
          align: 'center',
          sortable: false,
          value: 'created_at',
        },
        {
          text: 'Livros', //nome da coluna
          align: 'center', //alinhamento -center, end, start
          sortable: false, //se permite ordenação dos dados por essa coluna
          value: 'titulo', //é o dado que essa coluna vai receber
        },
        { text: "", value: "actions" },
      ],
      emprestimos: [],
      livros: []
    }
  },
  created () {
    this.getEmprestimos();
    this.getLivros();
  },
  methods: {
    async getLivros () {
      this.livros = await this.$axios.$get('http://localhost:3333/livros');
    },
    async getEmprestimos () {
      this.emprestimos = await this.$axios.$get('http://localhost:3333/emprestimos');
      this.emprestimos.forEach((emprestimo) => {
        let ano = emprestimo.created_at.substring(0, 4);
        let mes = emprestimo.created_at.substring(5, 7);
        let dia = emprestimo.created_at.substring(8, 10);
        emprestimo.created_at = `${ano}-${mes}-${dia}`
      });
    },
    async deletarItem (emprestimo) {
      try {
        if (confirm(`Deseja deletar o emprestimo id ${emprestimo.id}?`)) {
          let response = await this.$axios.$post('http://localhost:3333/emprestimos/deletar', { id: emprestimo.id });
          this.$toast.success(response.message)
          this.getEmprestimos();
          console.log('chegou');
        }
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao atender a requisição. Contate o administrador do sistema.')
      }
    },
        async encerrarEmprestimo (emprestimo) {
      try {
        if (confirm(`Deseja encerrar o emprestimo id ${emprestimo.id}?`)) {
          let response = await this.$axios.$post(`http://localhost:3333/emprestimos/${emprestimo.id}`, {devolucao:  new Date(Date.now()).toISOString().substring(0,10)});
          this.$toast.success(response.message)
          this.getEmprestimos();
        }
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao atender a requisição. Contate o administrador do sistema.')
      }
    },

    async editarItem(emprestimo) {
      this.$router.push({
        name: 'emprestimos-cadastro',
        params: { id: emprestimo.id }
        });
    }
  }
} 
</script>