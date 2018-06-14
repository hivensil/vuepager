<template>
  <div id="app">
    <Pager total="50" toPage="5"></Pager>	
  </div>
</template>

<script>
import Pager from './Pager.vue'
export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  components:{
	Pager
  },
  mounted(){
	this.$on('changePage',function(a){alert('Current page is:'+a+'. The numbering starts from 0')});
  },
  methods:{
	changePage(a){
		alert(a);
	}
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h1, h2 {
  font-weight: normal;
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
