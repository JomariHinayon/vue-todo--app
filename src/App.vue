<template>
  <div class="flex flex-col h-screen w-screen justify-center items-center">
    <Header />
    <div class="flex">
      <input 
        type="text" name="inputTodo" placeholder="Add a task"
        class="inputTask border-[1px] focus:border-sky-200 border-sky-800 p-2 rounded-md"
        v-model="newTask"
        :class="{ 'border-b-4 border-red-500 drop-shadow-lg' : inputError}"
      />
      <button 
        @click="addTask()" 
        class="px-3 mx-3 rounded-md text-white bg-red-700"
        :class="{'bg-green-600' : taskEdit != null}"
      >
        {{taskEdit == null ? "Submit" : "Save"}}
      </button>
    </div>
      <div class="p-5">
        <table class="table-fixed">
							<thead class="bg-gray-200 border-b-2">
								<tr>
									<th class="p-2 w-80 text-left">Task</th>
									<th class="p-2">Status</th>
									<th class="p-2">Edit</th>
									<th class="p-2">Delete</th>
								</tr>
							</thead>
							<tbody>
								<tr v-for="(todo, index) in todoList" :key="index">
									<td class="py-2">{{todo.task}}</td>
									<td 
                    class="py-2 text-center cursor-pointer" 
                    @click="setStatus(index)"
                    :class="{'text-red-600' : todo.status === todoStatus[0],
                              'text-yellow-600' : todo.status === todoStatus[1],
                              'text-green-600' : todo.status === todoStatus[2]
                            }" 
                  >
                    {{
                      todo.status
                    }}
                    </td>
									<td  class="py-2 text-center">
                    <button @click="editTask(index)">
                      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6 hover:text-green-500">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M16.862 4.487l1.687-1.688a1.875 1.875 0 112.652 2.652L10.582 16.07a4.5 4.5 0 01-1.897 1.13L6 18l.8-2.685a4.5 4.5 0 011.13-1.897l8.932-8.931zm0 0L19.5 7.125M18 14v4.75A2.25 2.25 0 0115.75 21H5.25A2.25 2.25 0 013 18.75V8.25A2.25 2.25 0 015.25 6H10" />
                      </svg>
                    </button>
                  </td>
                  <td class="py-2 text-center">
                    <button @click="deleteTask(index)">
                      <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6 hover:text-red-500">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M14.74 9l-.346 9m-4.788 0L9.26 9m9.968-3.21c.342.052.682.107 1.022.166m-1.022-.165L18.16 19.673a2.25 2.25 0 01-2.244 2.077H8.084a2.25 2.25 0 01-2.244-2.077L4.772 5.79m14.456 0a48.108 48.108 0 00-3.478-.397m-12 .562c.34-.059.68-.114 1.022-.165m0 0a48.11 48.11 0 013.478-.397m7.5 0v-.916c0-1.18-.91-2.164-2.09-2.201a51.964 51.964 0 00-3.32 0c-1.18.037-2.09 1.022-2.09 2.201v.916m7.5 0a48.667 48.667 0 00-7.5 0" />
                      </svg>
                    </button>
                  </td>
								</tr>	
							</tbody>
						</table>
      </div>
  </div>
</template>

<script>
// export components
import Header from './components/Header.vue'

export default {
  name: 'App',
    data() {
      return {
        newTask: "",
        taskEdit: null,
        inputError: false,
        todoStatus: ["To-do", "In progress", "Finished"],
        todoList: [
          {
            task: "Wake up",
            status: "In progress"
          },
        ],
      }
    },
  components: {
    Header,
  },
  methods: {
    addTask() {
      if(this.taskEdit != null) {
        this.todoList[this.taskEdit].task = this.newTask
        this.taskEdit = null
        this.newTask = "";
      }else if(this.newTask.trim().length === 0){
        this.inputError = true;
      }else {this.todoList.push({
        task: this.newTask,
        status: "To-do"
        })
        this.newTask = ""
        this.inputError = false
      }
    },

    deleteTask(index) {
      this.todoList.splice(index, 1)
    },

    editTask(index) {
      this.newTask = this.todoList[index].task
      this.taskEdit = index
    },

    setStatus(index) {
      let getStatus = this.todoStatus.indexOf(this.todoList[index].status)
      getStatus++
      if(getStatus > 2) {
        getStatus = 0
      }
      this.todoList[index].status = this.todoStatus[getStatus]
      
    }
  },
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@100;200;300;400;500;600;700;800;900&display=swap');

*{
  box-sizing: border-box;
  padding: 0;
  margin: 0;
}

body {
  font-family: 'Montserrat', sans-serif;
}
</style>
