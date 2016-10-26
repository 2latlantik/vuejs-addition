<template>
  <td><input placeholder="chiffre" value="" v-on:keyup="modif" v-model="value1" :class="{green: this.value1 === this.input1}" maxlength="1" ></td>
</template>

<script>
  export default{
    data () {
      return {
        value1: ''
      }
    },
    props: ['input1', 'indice'],
    methods: {
      modif: function () {
        this.$emit('comm', this.value1, this.indice)
      }
    }
  }
</script>

<style>
  .green {
    background-color: #0ea432;
  }


</style>