<template>
  <div>
    <h1>BPM-PPQ-TICK</h1>
    <div>
      <p>BPM: <input v-model="bpm" type="number"></p>
      <p>midi分解能(ppq): <input v-model="ppq" type="number"></p>
      <p>1 bpm: {{bpm1}} ms</p>
      <p>1 tick: {{tick1.toFixed(2)}} ms</p>
      <p>10 tick: {{tick10.toFixed(2)}} ms</p>
    </div>
    <div>
      <table>
        <tr>
          <th>value</th>
          <th>tick</th>
          <th>ms</th>
        </tr>
        <tr v-for="row in rows" v-bind:key="row.title">
          <td>{{row.title}}</td>
          <td>{{row.tick}}</td>
          <td>{{row.ms}}</td>
        </tr>
      </table>
    </div>
    <p>
      計算間違ってたらごめんなさい。issueに起票お願いします↓<br/>
      <a href="https://github.com/modeverv/bpm_ppq_tick/issues">issue</a>
    </p>
  </div>
</template>

<script>

export default {
  name: 'App',
  data: () => {
    return {
      bpm: 60,
      ppq: 480,
    }
  },
  computed: {
    bpm1() {
      let result = NaN
      try{
        result = 60 * 1000 / this.bpm
      } catch (e) {
        // noop
      }
      return result
    },
    tick1() {
      let result = NaN
      try{
        result = 60 / this.bpm / this.ppq * 1000
      } catch (e) {
        // noop
      }
      return result
    },
    tick10() {
      let result = NaN
      try{
        result = 60 / this.bpm / this.ppq * 1000 * 10
      } catch (e) {
        // noop
      }
      return result
    },
    rows() {
      let define = [
        {title: "1", tick: NaN, ms: NaN, arg: 0.25},
        {title: "1/2", tick: NaN, ms: NaN, arg: 0.5},
        {title: "1/4", tick: NaN, ms: NaN, arg: 1},
        {title: "1/8", tick: NaN, ms: NaN, arg: 2},
        {title: "1/16", tick: NaN, ms: NaN, arg: 4},
        {title: "1/32", tick: NaN, ms: NaN, arg: 8},
        {title: "1/64", tick: NaN, ms: NaN, arg: 16},
        {title: "1/128", tick: NaN, ms: NaN, arg: 32},
        {title: "1/256", tick: NaN, ms: NaN, arg: 64},
        {title: "1/2T", tick: NaN, ms: NaN, arg: 0.75},
        {title: "1/4T", tick: NaN, ms: NaN, arg: 1.5},
        {title: "1/8T", tick: NaN, ms: NaN, arg: 3},
        {title: "1/16T", tick: NaN, ms: NaN, arg: 6},
      ]
      define.forEach(function(r){
        try {
          r.tick = (this.ppq / r.arg).toFixed(2)
        } catch (e) {
          // noop
        }
        try {
          r.ms = (r.tick * this.tick1).toFixed(2)
        } catch (e) {
          // noop
        }
      },this)
      return define
    }
  },
  components: {
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
table {
  margin: 0 auto;
}
table, th,td {
  text-align: center;
  border-collapse: collapse;
  border:1px solid #000;
}
</style>
