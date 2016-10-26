<template>
  <tr class="table__addition__result">
    <td><i class="calculator icon" aria-hidden="true"></i></td>
    <ResultInput v-on:comm="comm" v-for="input1 in resultExplode" :input1="input1.value" :indice="input1.id"></ResultInput>
    <td></td>
  </tr>
</template>



<script>
  import ResultInput from './ResultInput.vue'
  export default {
    components: { ResultInput },
    props: ['resultat'],
    data: function () {
      return {
        resultLineState: false
      }
    },
    methods: {
      comm: function (val, indice) {
        for (let i = 0; i < this.resultExplode.length; i++) {
          if (indice === this.resultExplode[i]['id']) {
            if (val === this.resultExplode[i]['value']) {
              this.resultExplode[i]['resultatCorrect'] = true
            } else {
              this.resultExplode[i]['resultatCorrect'] = false
            }
          }
        }
        this.checkResultState()
      },
      checkResultState: function () {
        let resultOk = true
        for (let i = 0; i < this.resultExplode.length; i++) {
          if (this.resultExplode[i]['resultatCorrect'] === false) {
            resultOk = false
          }
        }
        this.$emit('resultState', resultOk)
      }
    },
    computed: {
      resultExplode: function () {
        let chaine = '' + this.resultat
        let objetReturn = []
        let lengthChaine = chaine.length
        for (let i = 0; i < lengthChaine; i++) {
          objetReturn.push({
            id: i,
            value: chaine[i],
            resultatCorrect: false
          })
        }
        return objetReturn
      }
    }
  }
</script>