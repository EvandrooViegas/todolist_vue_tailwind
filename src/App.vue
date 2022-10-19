<script setup>
    import {reactive, ref, computed, watch} from "vue"

    const taskArr = ref([
      {text: "Task 1", id: 1, isDone: true}
    ])
    const filtredTasks = ref(taskArr.value)

    const isHideFinishedTasks = ref(false)

    const text = ref("")

    const createTask = () => {
        if(text.value) {
            taskArr.value.push({  
                text: text.value,
                id: Math.random() * 10,
                isDone: false
            })
            text.value = ''
        }
    }

    const removeTask = (id) => {
      filtredTasks.value = taskArr.value = taskArr.value.filter(t => t.id !== id)
    }

    const setDoneTask = (id) => {
      filtredTasks.value = taskArr.value = taskArr.value.map((task) => {
            if(task.id == id) {
                task.isDone = !task.isDone
            }
            return task
        })
    }

    watch(isHideFinishedTasks, (nv) => {

      if(nv == false) {
        filtredTasks.value = taskArr.value
      } else {
        filtredTasks.value = filtredTasks.value.filter(t => !t.isDone)
      }
    })

    
    


</script>

<template>
  <div class="flex flex-col justify-around items-center h-[100vh]">

    <form @submit.prevent="createTask()" class="flex w-full gap-6 flex-col justify-around items-center">
        <h3 class="text-3xl font-semibold">What has to be made</h3>
        <input 
          class="rounded-md w-[70%] bg-neutral-700 outline-transparent border-none"
          type="text" 
          v-model="text"
        >


    </form>

    <div>
      <button class="bg-indigo-600 p-2 rounded-md" @click="() => {isHideFinishedTasks = !isHideFinishedTasks}">
        {{isHideFinishedTasks ? "Show finished tasks" : "Hide Finished tasks"}}
      </button>
    </div>

    <div
      class="flex flex-col h-[50%] items-center gap-6 w-[70%]"
    >
        <div 
          class="border-t-2 border-neutral-600 w-full p-2 flex justify-between items-center "
          v-for="task in filtredTasks"
          :key="task.id"  
        >
          <li 
              @click="setDoneTask(task.id)"
              @dblclick="removeTask(task.id)"

              :class="`${task.isDone == true && 'done'}`"
          >
              {{ task.text }}
          </li>

          <button @click="removeTask(task.id)" class="bg-red-400 p-2 rounded-sm"> 
            Delete
          </button>

      </div>
    </div>
      

  </div>
</template>


<style>
    li {
        user-select: none;
        list-style: none;
    }

    .done {
        text-decoration: line-through;
        
    }
</style>



