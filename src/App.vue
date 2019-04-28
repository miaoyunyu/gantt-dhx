<template>
  <div class="container">

     <!-- <div class="right-container">
      <div class="gantt-selected-info">
        <div v-if="selectedTask">
          <h2>{{selectedTask.text}}</h2>
          <span><b>ID: </b>{{selectedTask.id}}</span><br/>
          <span><b>Progress: </b>{{selectedTask.progress|toPercent}}%</span><br/>
          <span><b>Start Date: </b>{{selectedTask.start_date|niceDate}}</span><br/>
          <span><b>End Date: </b>{{selectedTask.end_date|niceDate}}</span><br/>
        </div>
        <div v-else class="select-task-prompt">
          <h2>Click any task</h2>
        </div>
      </div>
      <ul class="gantt-messages">
        <li class="gantt-message" v-for="message in messages">{{message}}</li>
      </ul>
    </div> -->

    <gantt class="left-container" 
          :tasks="tasks"
          @task-updated="logTaskUpdate"
          @link-updated="logLinkUpdate" 
          @task-selected="selectTask">
    </gantt>
  </div>
</template>

<script>
import Gantt from './components/Gantt.vue'
import { debug } from 'util';
import { constants } from 'crypto';

(function () {} ())

export default {
  name: 'app',
  components: {Gantt},
  data () {
    return {
      tasks: {
        data: [
        {id:1, text:"Project #1",    type:gantt.config.types.project,    open:true},   
        {id:2, text:"Task #1",       start_date:"12-04-2013", duration:3, parent:1},
        {id:3, text:"Alpha release", type:gantt.config.types.milestone,  open:true,  start_date:"14-04-2013"},                                                
        {id:4, text:"Task #2",       start_date:"17-04-2013", duration:3, parent:3}
        ],
      },
    selectedTask: null,
      messages: []
    }
  },
  filters: {
    toPercent (val) {
      if(!val) return '0'
      return Math.round((+val) * 100)
    },
    niceDate (obj){
      return `${obj.getFullYear()} / ${obj.getMonth() + 1} / ${obj.getDate()}`
    }
  },
  methods: {
    selectTask (task) {
      this.selectedTask = task
    },
  
    addMessage (message) {
      
      this.messages.unshift(message)
      if(this.messages.length > 40) {
        this.messages.pop()
      }
    },
    //拖动事件，修改时间
    logTaskUpdate (id, mode, task) {
      debugger

      console.log(this.selectedTask)
      
      let text = (task && task.text ? ` (${task.text})`: '')
      let message = `Task ${mode}: ${id} ${text}` 
      this.addMessage(message)
      console.log(this.selectedTask)
    },

    logLinkUpdate (id, mode, link) {
      
      let message = `Link ${mode}: ${id}`
      if(link){
        message += ` ( source: ${link.source}, target: ${link.target} )`
      }
      this.addMessage(message)
       console.log(this.tasks)
    }
  }
}


</script>

<style>
  html, body {
    height: 100%;
    margin: 0;
    padding: 0;
  }

  .container {
    height: 100%;
    width: 100%;
  }

  .left-container {
    overflow: hidden;
    position: relative;
    height: 100%;
  }

  .right-container {
    border-right: 1px solid #cecece;
    float: right;
    height: 100%;
    width: 340px;
    box-shadow: 0 0 5px 2px #aaa;
    position: relative;
    z-index:2;
  }

  .gantt-messages {
    list-style-type: none;
    height: 50%;
    margin: 0;
    overflow-x: hidden;
    overflow-y: auto;
    padding-left: 5px;
  }

  .gantt-messages > .gantt-message {
    background-color: #f4f4f4;
    box-shadow:inset 5px 0 #d69000;
    font-family: Geneva, Arial, Helvetica, sans-serif;
    font-size: 14px;
    margin: 5px 0;
    padding: 8px 0 8px 10px;
  }

  .gantt-selected-info {
  border-bottom: 1px solid #cecece;
    box-sizing: border-box;
    font-family: Geneva, Arial, Helvetica, sans-serif;
    height: 50%;
    line-height: 28px;
    padding: 10px;
  }

  .gantt-selected-info h2 {
    border-bottom: 1px solid #cecece;
  }
  
  .select-task-prompt h2{
    color: #d9d9d9;
  }
</style>
