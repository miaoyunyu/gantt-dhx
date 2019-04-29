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
        {name:"text",       label:"任务名称",  width:"*", tree:true },
        {name:"start_date", label:"开始时间", align:"center", width:90 },
        {name:"end_date",   label:"结束时间",  align:"center" , width:90},
         // {name:"add",        label:"",           width:44 }
      ];
     gantt.locale = {
        date:{
            month_full:["一月", "二月", "三月", "四月", "五月", "六月", "七月", "八月", "九月",
            "十月", "十一月", "十二月"],
            month_short:["一月", "二月", "三月", "四月", "五月", "六月", "七月", "八月", "九月",
            "十月", "十一月", "十二月"],
            day_full:["星期日", "星期一", "星期二", "星期三", "星期四", "星期五",
            "星期六"],
            day_short:["日", "一", "二", "三", "四", "五", "六"]
        },
        labels:{ 
            icon_save:"保存",
            icon_cancel:"取消",
            icon_details:"详情",
            icon_edit:"Edit",
            icon_delete:"册除",
            section_description:"任务名称",
            section_time:"任务时间",
            minutes: "分",
            hours: "小时",
            days: "日",
            weeks: "周",
            months: "月",
            years: "年"
        }
    };
  }
}
</script>

<style>
  @import "~dhtmlx-gantt/codebase/dhtmlxgantt.css";
</style>