<template>
  <div id="app">
    <div class="ui stacked segment">
      <h2> Placer un nouveau chiffre dans l'addition </h2>
      <input id="nouvel-operande" placeholder="Nouveau chiffre" v-model="nouvelOperande" @keyup.enter="addOperande">
    </div>
    <div class="ui stacked segment" v-if="operandesFormated.length > 0">
      <h2> Résoudre l'addition </h2>
      <table class="table__addition" border="1">
        <OperandeLine v-for="(operande , operandeIndex) in operandesFormated" :operande="operande"
                      :operandeIndex="operandeIndex" v-on:deleteOperande="deleteOperande"></OperandeLine>
        <ResultLine :resultat="resultat" v-on:resultState="resultState"></ResultLine>
      </table>
      <div class="ui positive message" v-if="result_state">
        <div class="header"> Bravo</div>
        <p> Vous avez résolu l'addition </p>
      </div>
    </div>
  </div>
</template>

<script>
  import OperandeLine from './components/OperandeLine'
  import ResultLine from './components/ResultLine'

  export default {
    name: 'app',
    components: {
      OperandeLine, ResultLine
    },
    data: function () {
      return {
        nouvelOperande: '',
        operandes: [],
        operandesMaxLength: 0,
        result_state: false
      }
    },
    methods: {
      setOperandesMaxLength: function (result) {
        let chaine = '' + result
        this.operandesMaxLength = chaine.length
      },
      addOperande: function () {
        if (!isNaN(parseFloat(this.nouvelOperande)) && isFinite(this.nouvelOperande)) {
          this.operandes.push(parseInt(this.nouvelOperande))
          if (this.nouvelOperande !== 0) {
            this.result_state = false
          }
        }
        this.nouvelOperande = ''
      },
      resultState: function (state) {
        this.result_state = state
      },
      deleteOperande: function (operandeSuppr) {
        operandeSuppr = parseInt(operandeSuppr.trim())
        let newArray = []
        let unRetrait = false
        for (let i = 0; i < this.operandes.length; i++) {
          if (this.operandes[i] !== operandeSuppr || unRetrait === true) {
            newArray.push(this.operandes[i])
          }
          if (this.operandes[i] === operandeSuppr) {
            unRetrait = true
          }
        }
        this.operandes = newArray
      }
    },
    computed: {
      operandesFormated: function () {
        let operandeEqualLength = []
        var self = this
        this.operandes.forEach(function (element) {
          let dataAdd = '' + element
          let spaces = ''
          if (dataAdd.length < self.operandesMaxLength) {
            for (let i = 0; i < self.operandesMaxLength - dataAdd.length; i++) {
              spaces = spaces + ' '
            }
          }
          dataAdd = spaces + dataAdd
          operandeEqualLength.push(dataAdd)
        })
        return operandeEqualLength
      },
      resultat: function () {
        let result = this.operandes.reduce((a, b) => a + b, 0)
        this.setOperandesMaxLength(result)
        return result
      }
    }
  }
</script>

<style>
  body {
    font: 14px 'Helvetica Neue', Helvetica, Arial, sans-serif;
    line-height: 1.4em;
    background: #f5f5f5;
    color: #4d4d4d;
    width:90%;
    padding-top: 20px;
    margin: 0 auto;
  }

  h2 {
    font-size: 25px;
    color: #0d71bb;
    padding:0 10px;
  }



  #nouvel-operande {
    position: relative;
    margin: 15px 20px;
    width: 80%;
    font-size: 24px;
    outline: none;
    padding: 15px 15px 15px 25px;
    border: none;
    box-shadow: inset -2px -2px 6px 0 rgba(0, 0, 0, 0.4);
    box-sizing: border-box;
    -webkit-font-smoothing: antialiased;
    -moz-font-smoothing: antialiased;
    font-smoothing: antialiased;
  }

  .table__addition {
    margin: 0 0 0 25px;
    width: 80%;
    text-align: center;
    border-collapse: collapse;
    border-spacing: 0;
    border:1px solid #cbcbcb;
    font-size: 18px;
  }

  .table__addition tr:nth-child(even) {
    background-color: #f2f2f2;
  }

  .table__addition tr:nth-child(odd) {
    background-color: #ffffff;
  }

  .table__addition tr td:first-child {
    width:30px;
  }

  .table__addition td{
    border-left: 1px solid #cbcbcb;
    border-width: 0 0 0 1px;
    font-size: inherit;
    margin: 0;
    overflow: visible;
    padding: .5em 1em;
  }

  .table__addition input{
    height: 40px;
    width: 60px;
    text-align: center;
    border: none;
    font-size: 20px;
  }

  .table__addition input:focus {
    outline: none;
    border-color: #9ecaed;
    box-shadow: inset 0 0px 8px #9ecaed;
  }

  .table__addition__result td{
    background-color: #dadada;
  }
</style>
