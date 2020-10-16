<template>
  <div id="app">
    <ClientForm />
  </div>
</template>

<script>
import ClientForm from './components/ClientForm';
export default {
  name: "App",
  components: {
    ClientForm
  },
};
</script>

<style lang="scss"> @import '@/assets/scss/style.scss'; </style>
