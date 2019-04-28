<template>
  <div ref="gantt"></div>
</template>

<script>
import 'dhtmlx-gantt'

export default {
  name: 'gantt',
  props: {
    tasks: {
      type: Object,
      default () {
        return {data: [], links: []}
      }
    }
  },

  methods: {
    $_initGanttEvents: function () {
      if(gantt.$_eventsInitialized)
        return;
      
      
     

      gantt.attachEvent('onTaskSelected', (id) => {
        let task = gantt.getTask(id)
        this.$emit('task-selected', task)
      })

      gantt.attachEvent('onAfterTaskAdd', (id, task) => {
        this.$emit('task-updated', id, 'inserted', task)
        task.progress = task.progress || 0
        if(gantt.getSelectedId() == id) {
          this.$emit('task-selected', task)
        }
      })

      gantt.attachEvent('onAfterTaskUpdate', (id, task) => {
        this.$emit('task-updated', id, 'updated', task)
      })

      gantt.attachEvent('onAfterTaskDelete', (id) => {
        this.$emit('task-updated', id, 'deleted')
        if(!gantt.getSelectedId()) {
          this.$emit('task-selected', null)
        }
      })

      gantt.attachEvent('onAfterLinkAdd', (id, link) => {
        this.$emit('link-updated', id, 'inserted', link)
      })

      gantt.attachEvent('onAfterLinkUpdate', (id, link) => {
        this.$emit('link-updated', id, 'updated', link)
      })

      gantt.attachEvent('onAfterLinkDelete', (id, link) => {
        this.$emit('link-updated', id, 'deleted')
      })
      gantt.$_eventsInitialized = true;
    }
  },
    
  mounted () {
    this.$_initGanttEvents();

    gantt.init(this.$refs.gantt)
    gantt.parse(this.$props.tasks)
  },
  created:function(){

    gantt.config.columns = [
    {name:"text",       label:"www name",  width:"*", tree:true },
    {name:"start_date", label:"Start time", align:"center" },
    {name:"duration",   label:"Duration",   align:"center" },
    {name:"add",        label:"",           width:44 }
];
    //  gantt.locale = {
    //     date:{
    //         month_full:["January", "February", "March", "April", "May", "June", "July",
    //         "August", "September", "October", "November", "December"],
    //         month_short:["一月", "二月", "三月", "四月", "五月", "Jun", "Jul", "Aug", "Sep",
    //         "Oct", "Nov", "Dec"],
    //         day_full:["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday",
    //         "Saturday"],
    //         day_short:["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"]
    //     },
    //     labels:{
    //     new_task:"New fdsfdsfsfs",
    //     icon_save:"Save",
    //     icon_cancel:"Cancel",
    //     icon_details:"Details",
    //     icon_edit:"Edit",
    //     icon_delete:"Delete",
    //     confirm_closing:"",//Your changes will be lost, are you sure ?
    //     confirm_deleting:"Task will be deleted permanently, are you sure?",
 
    //     section_description:"Description",
    //     section_time:"Time period",
 
    //     /* link confirmation */
 
    //     confirm_link_deleting:"Dependency will be deleted permanently, are you sure?",
    //     link_from: "From",
    //     link_to: "To",
    //     link_start: "Start",
    //     link_end: "End",
 
    //     minutes: "Minutes",
    //     hours: "Hours",
    //     days: "Days",
    //     weeks: "Week",
    //     months: "Months",
    //     years: "Years"
    // }
    // };
  }
}
</script>

<style>
  @import "~dhtmlx-gantt/codebase/dhtmlxgantt.css";
</style>