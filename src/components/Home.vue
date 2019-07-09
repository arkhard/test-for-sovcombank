<template lang="pug">
	.Home
		.Home-test
			input( type="text" v-model="test")
			p {{test}}
		search
</template>

<script>
  import Search from './Search';

export default {
  name: 'HelloWorld',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
	  test: 'hello'
    }
  },
  components: {
    Search
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
 .Home {
   display: flex;
   justify-items: center;
 }
 .Search {
   margin: auto;
   width: 40%;
 }
a {
  color: #42b983;
}
</style>
