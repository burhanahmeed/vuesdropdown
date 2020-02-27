<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <span>By <a href="http://kusiaga.com" target="_blank">Kusiaga</a></span>
    <p>Vuesdropdown is a select dropdown component that unique and different from other existing components.</p>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
