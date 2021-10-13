<template>
  <v-container>
    <v-row class="text-center mb-10">
      <v-col md="6" class="mt-4">
        <v-text-field
          ref="taskInput"
          label="Enter task"         
          v-model="task"        
          @keyup.enter="addTask"
        ></v-text-field>        
      </v-col>
      <v-col md="1" class="mt-4">
        <v-btn
          color="primary"
          class="ma-3 white--text"
          @click="addTask()"
        >
          Add task
          <v-icon
            right
            dark
          >
            mdi-plus-circle
          </v-icon>
        </v-btn>        
      </v-col>      
    </v-row>
  </v-container >
</template>

<script>
  export default {
    name: 'InputTask',
  data () {
    return {
      task: null,
      itemsStatus: [
        { id: 1, title:'Todo' }, 
        { id: 2, title:'Process' },
        { id: 3, title:'Finished' }],
      }
    },
    mounted() {
      this.$refs.taskInput.focus();
    },     
    methods: {
      addTask() {
        if (this.task) {                    
          let newTask = {
            // id: this.todoList.length + 1,
            id: Date.now() + Math.trunc(Math.random()),
            selectedStatus: { id: 1, title:'Todo' }, 
            task: this.task,
            itemsStatus: this.itemsStatus
          }
          this.task = null
          this.$refs.taskInput.focus(); 
          this.$emit('addTask', newTask);
          }
        }
    }
  }
</script>
