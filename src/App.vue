<template>
  <div id="app">
    <myheader></myheader>
    <div>
      <p v-if="msg.length > 0">
        {{ msg }}
      </p>
      <p v-else>テキストを入れてね</p>
    </div>
    <input type="text" v-model="msg">
    <button @click="clear()">clear</button>
    <button @click="geoName()">geoName</button>

    <!--<img src="./assets/logo.png">-->
    <router-view/>
    <div style="margin: 20px;">
      <form v-on:submit.prevent="addTimeline">
      <input type="text" v-model="newMessage">
      <button type="submit">発言する</button>
      </form>
    </div>
    <timeline>
      <timeline-title>タイムライン</timeline-title>
      <timeline-item color="red">言論</timeline-item>
      <timeline-item :hollow="true">2018.09.01 14:00 コーチングセッション予約中</timeline-item>
      <timeline-item v-for="tlm in tlMessages" color="blue">{{ tlm }}</timeline-item>
    </timeline>
  </div>
</template>

<script>
import myheader from './components/myheader'
import { Timeline, TimelineItem, TimelineTitle } from 'vue-cute-timeline'

export default {
  name: 'App',
  components: {
    myheader,
    Timeline,
    TimelineItem,
    TimelineTitle
  },
  data () {
    return {
      msg: 'Hello World.',
      newMessage: '',
      tlMessages: []
    }
  },
  methods: {
    clear () {
      this.msg = ''
    },
    geoName () {
      var that = this
      $.getJSON('http://www.geonames.org/postalCodeLookupJSON?postalcode=10504&country=US&callback=?', {}, function (json) {
        console.log(json)
        that.msg = json.postalcodes[0].adminName1
      })
    },
    addTimeline () {
      var that = this
      that.tlMessages.push(that.newMessage)
    }
  },
  ready () {
    var that = this
    $.getJSON('http://www.geonames.org/postalCodeLookupJSON?postalcode=10504&country=US&callback=?', {}, function (json) {
      console.log(json)
      that.msg = json.postalcodes[0].adminName1
    })
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

.timeline {
  width: 50%;
  margin: auto;
}
</style>
