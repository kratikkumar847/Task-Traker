<template>
        <div v-for="Task1 in Tasks" :key="Task1.id">
            <singleTask @toggle-reminder="$emit('toggle-reminder', Task1.id)" @delete-task="$emit('delete-task',Task1.id)" :task = "Task1" />
        </div>
          
</template>


<script>
import singleTask from "./singleTask"

    export default
     {
        name : 'Tasks',
        props:{
            Tasks : Array
        },
        components:{
            singleTask
        },
        emits:['delete-task', 'toggle-reminder']
     }
</script>