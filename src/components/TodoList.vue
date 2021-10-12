<template>
  <v-container>
    <v-dialog v-if="dialog"
      v-model="dialog"
      persistent
      max-width="600px"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="primary"
          dark
          v-bind="attrs"
          v-on="on"
        >
          Open Dialog
        </v-btn>
      </template>
      <v-card>
        <v-card-title>
          <span class="text-h5">Modify task</span>
        </v-card-title>
        <v-card-text>
          <v-container>
            <v-row>
              <v-col
                cols="12"
                sm="12"                
              >
                <v-text-field
                  label="Task"  
                  filled              
                  clearable                
                  v-model="modalTaskValue"
                  required
                ></v-text-field>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn
            color="blue darken-1"
            text
            @click="dialog = false"
          >
            Close
          </v-btn>
          <v-btn
            color="blue darken-1"
            text
            @click="saveModalChanges"
          >
            Save
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <input-task @addTask="addTask"></input-task>
    <v-row class="text-center mt-2">
      <v-col md='10'>        
        <v-data-table         
          dense
          :headers="headers"
          :items="todoList"
          :items-per-page="5"
          :search="search"
          class="elevation-4 mt-4"
        >     
        <template v-slot:top>
          <v-text-field
            v-model="search"
            label="Search (UPPER CASE ONLY)"
            class="mx-4 md-8"
          ></v-text-field>
        </template>
        <template v-slot:[`item.itemsStatus`]="{ item }">          
          <v-select     
            v-model="item.selectedStatus"
            :items="item.itemsStatus"
            item-text="title"
            item-value="id"
            return-object
            class="md-2"
          ></v-select>                                      
        </template>             
        <template v-slot:[`item.icon`]="{ item }">
          <v-icon class="md-1" @click="editTask(item)">mdi-pencil</v-icon>
          <v-icon class="md-1" @click="deleteTask(item)">mdi-delete</v-icon>
        </template>
        </v-data-table>
      </v-col>      
    </v-row>           
  </v-container >
</template>
<script>

  import InputTask from '../components/InputTask.vue'  

  export default {
    name: 'TodoList',
    components: {
      InputTask
    },    
    data () {
      return {
        modalTaskValue: null,
        selectedIndex: null,
        dialog: false,
        itemsStatus: [{ id: 1, title:'Todo' }, 
                { id: 2, title:'Process' },
                { id: 3, title:'Finished' }],
        selectedStatus: { id: 1, title: 'Todo' },                         
        search: '',         
        headers: [     
        { text: 'Task', align: 'left', value: 'task', sortable: true },                
        { text: 'Status', align: 'left', value: 'itemsStatus', sortable: false },                
        { text: 'Icon', align: 'left', value: 'icon', sortable: false }],
        todoList: [{
          id: 1,
          task: 'Comprar comida semanal',
          selectedAction: { value: null }
        },
        {
          id: 2,        
          task: 'Llevar el coche al taller',
          selectedAction: { id: 1, title: 'Edit' }
        }]         
      }
    },
    watch: {
      todoList: {
        handler: function(updatedTodoList) {          
          localStorage.setItem('todo_list', JSON.stringify(updatedTodoList));
        },
        deep: true
      }
    },    
    mounted() {
      this.getTodoList();
    },    
    methods: {
      addTask(item) {
        this.todoList.push(item)       
      },
      deleteTask(item) {     
        this.todoList = this.todoList.filter((todoItem) => todoItem.id !== item.id)          
      },
      editTask(item) {
        console.log('Has seleccionado editTask: ', item)        
        this.selectedIndex = this.todoList.map(e => e.id).indexOf(item.id);
        this.modalTaskValue=this.todoList[this.selectedIndex].task
        console.log('this.selectedIndex: ', this.selectedIndex)
        this.dialog=true
      },
      getTodoList() {
        if (localStorage.getItem('todo_list')) {
          this.todoList = JSON.parse(localStorage.getItem('todo_list'));          
        } else { 
          this.todoList = this.todoList.map(item=> ({ ...item, itemsStatus: this.itemsStatus, selectedStatus: this.selectedStatus }))
        }
      },
      saveModalChanges() {
        this.todoList[this.selectedIndex].task = this.modalTaskValue
        this.dialog = false
      }
    }
  }
</script>
