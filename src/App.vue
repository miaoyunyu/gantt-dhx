<template>
  <div class="container">
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


export default {
  name: 'app',
  components: {Gantt},
  data () {
    return {
      tasks: {
        data: [],
        links:[],
      },
    selectedTask: null,
      messages: []
    }
  },
  methods: {
    selectTask (task) {
      this.selectedTask = task
    },
    //tasks
    logTaskUpdate (id, mode, task) {
      debugger
      let text = (task && task.text ? ` (${task.text})`: '')
      let message = `Task ${mode}: ${id} ${text}` 
    },
    //link
    logLinkUpdate (id, mode, link) {
      let message = `Link ${mode}: ${id}`
      if(link){
        message += ` ( source: ${link.source}, target: ${link.target} )`
      }
    },  // 获取数据
    getData: function() {     
      //  axios.get('/projects/gantt_chart/'+'459c0426bbac407fb9d9de9ebbe53514')
      //        .then(res => {
      //           this.regroupData(res)
      //   })


      let res={
         result:1,
         data:[
             {id:1, text:"任务1", type:gantt.config.types.milestone,   start_date:1556519957000, end_date:1559111957000,open:true},
             {id:2, text:"任务2", type:gantt.config.types.milestone,   start_date:1556519957000, end_date:1559111957000, parent:1},
         ]
      }
       this.regroupData(res)
     },
    // 设置数据填充表格
     regroupData:function(data){
        if(data.result == 1){
            let task=this.tasks.data
            let listData=data.data
            listData.forEach(item=>{
                   let cur={
                        id: item.id,
                        publicId:item.publicId,
                        text: item.text,
                        user: item.user,
                        type: item.type,
                        start_date: new Date(item.start_date),
                        end_date: new Date(item.end_date),
                        parent:item.parent,
                        open:item.open                      
                   }
                  task.push(cur)
            })
       } 
     },
    
  },
  created:function(){
        this.getData();
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
