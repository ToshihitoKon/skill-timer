<template>
  <el-container style="text-align: center">
    <el-main>
    <el-header>{{songDelta}}</el-header>
      <el-row :gutter="20" style="width: 100%">
        <el-col :span="4" v-for="skill, index in skills" :key="index">
          <p>{{skill.type}} {{skill.duration}}s</p>
          <img alt="Vue logo" style="width:100%" src="./assets/logo.png" v-if="skill.state">
          <div>
            <span>skill type</span>
            <el-input style="width:100px" v-model="skill.type"></el-input>
          </div>
          <div>
            <span>interval</span>
            <el-input style="width:50px" v-model="skill.interval"></el-input>
            <span>s</span>
          </div>
          <div>
            <span>duration</span>
            <el-input style="width:50px" v-model="skill.duration"></el-input>
            <span>s</span>
          </div>
        </el-col>
      </el-row>
      <el-button type="success" v-on:click="start">スタート</el-button>
      <el-button type="warning" v-on:click="stop">リセット</el-button>
      <div>
        debug
        <el-button v-on:click="skillStart(1)">start</el-button>
      </div>
    </el-main>
  </el-container>
</template>

<script>

export default {
  name: 'App',
  data: function(){
    return {
      skills: [
        {type: "", interval: 9, duration: 6, state: false},
        {type: "", interval: 9, duration: 6, state: false},
        {type: "", interval: 9, duration: 6, state: false},
        {type: "", interval: 9, duration: 6, state: false},
        {type: "", interval: 9, duration: 6, state: false},
        {type: "", interval: 9, duration: 6, state: false},
      ],
      songStartTime: 0,
      now: 0,
    }
  },
  mounted () {
    this.watchNowLoop()
  },
  computed:{
    songDelta: function(){
      return this.toDisplayTime( this.now - this.songStartTime)
    },
  },
  methods:{
    watchNowLoop: function() {
      window.requestAnimationFrame(this.watchNowLoop)
      this.watchNow()
    },
    watchNow: function () {
      this.now = Date.now()
    },
    start: function () {
      this.songStartTime = new Date()
      var timerIdList = []
      for (var i=0; i<this.skills.length; i++){
        timerIdList.push(setInterval(this.skillStart, this.skills[i].interval * 1000, i))
      }
      this.timerIdList = timerIdList
      console.log("timer start", timerIdList)
    },
    stop: function () {
      const timerIdList = this.timerIdList
      for (var i=0; i<timerIdList.length; i++){
        clearInterval(timerIdList[i])
      } 
      this.timerIdList = []
      console.log("timer stop", timerIdList)
    },
    skillStart: function(index){
      console.log(index)
      const skills = this.skills
      skills[index].state = true
      this.skills = skills
      setTimeout(function(){
        skills[index].state = false
        this.skills = skills
      }, skills[index].duration * 1000)
    },
    toDisplayTime: function(msec) {
      return ( msec / 1000 ).toFixed(2)
    },
  },
}
</script>
