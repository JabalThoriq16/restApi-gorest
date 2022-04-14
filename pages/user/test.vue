<template>
  <div id="app">
    <!-- {{ info }} -->
  </div>
</template>
<script>
export default {
  data() {
    return {
      info: null,
    }
  },
  mounted() {
    axios
      .get('https://gorest.co.in/public/v2/users.json')
      .then((response) => (this.info = response))
  },
}
</script>
