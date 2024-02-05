<template>
  <div id="app">
    <Calc :title="message" v-on:result-event="appAction"/>
    <div class = "mt-3 text-left">
      <table class="table" v-html="log"></table>
    </div>
    <div>
      <button class="btn btn-danger" v-on:click="doClear">Clear log</button>
    </div>
  </div>
</template>

<script>
import Calc from './components/Calc.vue'

export default {
  name: 'App',
  components: {
    Calc
  },
  data() {
    return {
      message: 'CALC',
      result: [],
    }
  },
  computed: {
    log() {
      let table = `<tr><th>Expression</th><th>Value</th></tr>`
      if (this.result.length > 0){
        for(var i in this.result) {
          table += '<tr><td>' + this.result[i][0] + '</td><th>' + this.result[i][0] + '</th></tr>'
        }
      }
      return table
    }
  },
  created() {
    let items = localStorage.getItem('log')
    let logs = JSON.parse(items)
    if(logs != null) {
      this.result = logs
    }
  },
  methods: {
    appAction(exp, res){
      this.result.unshift([exp, res])
      var log = JSON.stringify(this.result)
      localStorage.setItem('log', log)
    },
    doClear() {
      if(confirm('ログを全て消します.')) {
        localStorage.removeItem('log')
        this.result = []
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
