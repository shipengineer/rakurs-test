<template>
  <div id="app" >
   <FormLayout>
    
   </FormLayout>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator';
import FormLayout from '@/layouts/FormLayout.vue'
@Component({
  components: {
    FormLayout,  
  },
})
export default class App extends Vue {}
</script>

<style lang="scss">
@import '@/assets/defaultStyles.scss';

#app{
  font-family: var(--font-family)
}
</style>
