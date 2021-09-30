<template>
  <div id="app" class="h-100 w-100">
    <RollerPageContainer/>
  </div>
</template>

<script>
import RollerPageContainer from './components/RollerPageContainer.vue'

export default {
  name: 'App',
  components: {
    RollerPageContainer
  }
}
</script>

<style>
 html, body {
   height: 100%;
   overflow-y: auto;
 }
</style>