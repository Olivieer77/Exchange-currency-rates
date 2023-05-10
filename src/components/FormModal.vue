<template>
  <div class="modal-container">
    <b-modal ref="modal" hide-footer>
      <div class="d-block text-center">
        <h3>Currency: {{currency }} </h3>
        <h3>Value: {{currencyValue}}</h3>
        <h3>Date: {{date}}</h3>
      </div>
      <b-button class="mt-3" block @click="$refs.modal.hide()">Close Me</b-button>
    </b-modal>
  </div>
</template>

<script>
// import ButtonsComp from "../components/ButtonsComp.vue"
export default {
  props: ['currency', 'date', 'currencyValue'],



}
</script>

<style scoped>
.modal-container {
  margin-top: 2.5rem;
}
</style>