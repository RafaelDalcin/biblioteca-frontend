<template>  
<v-container
  class="ma-2">
    <v-row>
       <v-col
          cols="5"
          md="4"
        >
          <v-text-field
            v-model="valorAtual"
            label=""
            outlined
          ></v-text-field>
        </v-col>
    </v-row>
  <v-row>
    <v-col>
          <v-btn
          elevation="2"
          tile
          @click="botaoCutucado('7')"
          > 7
          </v-btn>
          <v-btn
          elevation="2"
          tile
          @click="botaoCutucado('8')"
          > 8
          </v-btn>
          <v-btn
          elevation="2"
          tile
          @click="botaoCutucado('9')"
          > 9
          </v-btn>
          <v-btn
          elevation="2"
          tile
          @click="botaoCutucado('+')"
          > +
          </v-btn>
      </v-col>
    </v-row>
    <v-row>
      <v-col>
        <v-btn
          elevation="2"
          tile
          @click="botaoCutucado('4')"
          > 4
          </v-btn>
          <v-btn
          elevation="2"
          tile
          @click="botaoCutucado('5')"
          > 5
          </v-btn>
          <v-btn
          elevation="2"
          tile
          @click="botaoCutucado('6')"
          > 6
          </v-btn>
          <v-btn
          elevation="2"
          tile
          @click="botaoCutucado('-')"
          > -
          </v-btn>
      </v-col>
    </v-row>
     <v-row>
      <v-col>
        <v-btn
          elevation="2"
          tile
          @click="botaoCutucado('1')"
          > 1
          </v-btn>
          <v-btn
          elevation="2"
          tile
          @click="botaoCutucado('2')"
          > 2
          </v-btn>
          <v-btn
          elevation="2"
          tile
          @click="botaoCutucado('3')"
          > 3
          </v-btn>
          <v-btn
          elevation="2"
          tile
          @click="botaoCutucado('/')"
          > /
          </v-btn>
      </v-col>
    </v-row>
     <v-row>
      <v-col>
        <v-btn
          elevation="2"
          tile
          @click="botaoCutucado('0')"
          > 0
          </v-btn>
          <v-btn
          elevation="2"
          tile
          @click="count"
          > =
          </v-btn>
          <v-btn
          @click="botaoCutucado('*')"
          elevation="2"
          tile
          > *
          </v-btn>
          <v-btn
          @click="limpar()"
          elevation="2"
          tile
          > LIMPAR
          </v-btn>
      </v-col>
    </v-row>
</v-container>

</template>

<script>
export default {
  name: 'CalculadoraCRSPage',
  data () {
    return {
      operador: null,
      visor: [],
      valorAtual: '',
      valor1: null,
      valor2: null,
      conta: [],
      numeros: ['1', '2', '3' ,'4' ,'5' ,'6' ,'7' ,'8' , '9', '0'],
      operacoes: ['+', '-', '*', '/'],
    }
  },


  methods: {
    botaoCutucado (value) {
    if(this.numbers.includes(value)){
      this.visor.push(value);      
      (this.valorAtual) = this.visor.join('')
      this.valorAtual = Number(this.valorAtual)
    }
    else {
      if (!this.valor1) {
        this.operador = value
        this.valor1 = this.valorAtual;
        console.log(this.valor1);
        this.valorAtual = null  
        this.visor = [null]
      }
      
    }

    },
    limpar () {
      this.valorAtual = ''
      this.visor = [null]
    },
    count () {
      switch (this.operador) {
        case '+':
          this.valor1 += this.valor2;
          break;
        case '-':
          this.valor1 -= this.valor2
          break;
        case '*':
          this.valor1 * this.valor2
          break;
        case '/':
          this.valor1 / this.valor2
          break;
        case '=':
          
          break
        default:
          break;
      }
    }
  }
}
</script>
