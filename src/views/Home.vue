<template>    
    <AddTask v-show="showAddTaskForm" @add-task="addTask"/>        
    <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks"/>
</template>

<script>
import Tasks from '../components/Tasks'
import AddTask from '../components/AddTask'

export default {
    name: 'Home',
    props: {
        showAddTaskForm: Boolean,
    },
    components: {
        Tasks,
        AddTask
    },
    data(){
        return{
            tasks: [],
            localdb: 'http://localhost:5000'
        }
    },
    methods:{
    async deleteTask(id){
      if(confirm("Are you sure?")){
        const res = await fetch(this.localdb+`/tasks/${id}`,
        {
          method: 'DELETE',
        });

        console.log('deleting task '+id);
        
        if(res.status===200)
          this.tasks = this.tasks.filter((task) => task.id !== id);
        else
          alert('Error deleting('+res.status+')');
      }
    },
    async toggleReminder(id){
      const taskToToggle = await this.fetchTask(id);
      const updTask = {...taskToToggle, reminder:!taskToToggle.reminder};
      const res = await fetch(`${this.localdb}/tasks/${id}`,{
        method: 'PUT',
        headers: {
          'Content-type':'application/json'
        },
        body: JSON.stringify(updTask)
      });
      const data = await res.json();

      this.tasks.forEach(task => {
          if(task.id==id){            
            task.reminder = !task.reminder;
          }       
      });
      console.log(`reminder ${id} toggled`);
    },
    async addTask(task){      
      const res = await fetch(`${this.localdb}/tasks`,{
        method: 'POST',
        headers: {
          'Content-type':'application/json',          
        },
        body: JSON.stringify(task)
      });

      const data = await res.json();

      this.tasks.push(data);

      this.toggleAddTaskForm();
    },
    toggleAddTaskForm(){
      console.log('toggling the add task form');
      this.showAddTaskForm=!this.showAddTaskForm;
    },
    async fetchTasks(){
      const res = await fetch(this.localdb+'/tasks');
      const data = await res.json();

      return data;
    },
    async fetchTask(id){
      const res = await fetch(`${this.localdb}/tasks/${id}`);

      const data = await res.json();

      return data;
    }
  },
  async created(){
    this.tasks = await this.fetchTasks();
  }
}
</script>