<template>
    <div v-if="showAddTask">
      <AddTask @add-task ='addTask' />
    </div>
    <Task 
        @toggle-reminder="toggleReminder" 
        @delete-task="deleteTask" 
        :Tasks="Tasks"
    />
</template>

<script>
      import Task from '../components/Task';
    import AddTask from '../components/AddTask';
    export default{
        name : 'Home',
        props :{
          showAddTask : Boolean
        },
        components:{
            Task,
            AddTask
        },
        data() {
          return {
            tasks :[]
          }
        },
        methods :{
    
            async addTask(task){
                const res= await fetch('api/tasks', {
                  method : 'POST',
                  headers :{
                    'Content-type' : 'application/json',
                    
                  },
                  body : JSON.stringify(task),
                })
                const data = await res.json();

                this.Tasks = [...this.Tasks, data];
            },
    
      async deleteTask(id){
        if(confirm("Are you sure ?")){
          const res= await fetch(`api/tasks/${id}` ,{
            method: 'DELETE',
          })
          res.status === 200 ? (this.Tasks = this.Tasks.filter((task) => task.id !==id)) : alert('Error Deleting task')
        }
        
      },
      async toggleReminder(id){

        const taskToToggle = await this.fetchTask(id)
        const updTask = {...taskToToggle, reminder:!taskToToggle.reminder}

        const res = await fetch(`api/tasks/${id}`,{
          method: 'PUT',
          headers:{
            'Content-type' : 'application/json'
          },
          body : JSON.stringify(updTask)
        })
        const data = await res.json();
        this.Tasks = this.Tasks.map((task) => task.id === id ? {...task, reminder: data.reminder} : task)
      },
      async fetchTasks(){
        const res= await fetch("api/tasks")
        const data = res.json();
        return data;
      },
      async fetchTask(id){
        const res= await fetch(`api/tasks/${id}`)
        const data = res.json();
        return data;
      }
    
  },
  async created(){
    this.Tasks = await this.fetchTasks();
  }
    }
</script>