<template>
  <q-layout view="lHh Lpr lFf">
    <q-page-container>
      <Login />
    </q-page-container>
  </q-layout>
</template>

<script>
import Login from './components/LoginV2.vue'
export default {
  name: 'LayoutDefault',
  components: {
    Login
  }
}
</script>

<style>
</style>
