<template>
  <div id="app">
   <FormLayout>
    <ActivationForm/>
   </FormLayout>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import FormLayout from '@/layouts/FormLayout.vue'
import ActivationForm from '@/components/ActivationForm.vue'
@Component({
  components: {
    FormLayout,
    ActivationForm
    
  },
})
export default class App extends Vue {}
</script>

<style lang="scss">

</style>
