<template>
  <tr>
    <td><i class="plus icon" v-if="operandeIndex > 0"></i></td>
    <td v-for="ope in operandeSplit">{{ ope }}</td>
    <td><i class="trash outline icon" aria-hidden="true" v-on:click="deleteOperande"></i></td>
  </tr>
</template>

<script>
  export default {
    props: ['operande', 'operandeIndex'],
    computed: {
      operandeSplit: function () {
        let chaine = '' + this.operande
        return chaine.split('')
      }
    },
    methods: {
      deleteOperande: function () {
        this.$emit('deleteOperande', this.operande)
      }
    }
  }
</script>

<style>

</style>