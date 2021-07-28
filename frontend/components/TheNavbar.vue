<template>
  <b-navbar>
    <template #brand>
      account: {{ account }}
    </template>
  </b-navbar>
</template>

<script>

  export default {
    name: 'TheNavbar',
    props: {
      account: {
        type: String,
        required: true,
      },
    },
    data () {
      return {
        valueName: '',
      }
    },
  }
</script>

<style scoped>
  .navbar {
    background: #2c3e50;
    color: #eee;
  }
</style>
