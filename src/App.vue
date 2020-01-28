<template>
  <div id="app">
    <userTable/>
  </div>
</template>

<script>
import userTable from './components/userTable.vue'

export default {
   name: 'app',
   components: {
      userTable
   },
  
   data(){
      return {
      }
   },
}
</script>

<style>
</style>
