<template>
  <div class="app">
    <div class="card-task">
      <input class="card-task-input" type="text" v-model="newTask.name" @keypress.enter="setTask">

      {{ newTask }}

      <div class="task" v-for="(task, index) in listTasks" :key="task.id">
        <input class="form-check-input" type="checkbox" @click="checkTask(index, task)">

        <div class="task-text" :class="{ 'texto-riscado': task.checked }">
          {{ task.name }}
        </div>

        <div @click="editTask(index, task)">
          ✏️
        </div>



      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',

  data() {
    return {
      newTask: this.initialNewTask(),
      listTasks: []
    }
  },

  methods: {
    initialNewTask() {
      return { id: null, name: '', checked: false , indexEditing: null, isEditing: false }
    },

    setTaskEditing({ id, name, checked, indexEditing }) {
      const newListTask = [...this.listTasks]
      newListTask.splice(indexEditing, 1, { id, name, checked  })
      this.listTasks = newListTask

      this.newTask = this.initialNewTask()
    },

    setNewTask(name) {
      const newId = this.listTasks.length === 0 ? 0 : [...this.listTasks].pop().id + 1

      this.listTasks.push({
        id: newId,
        name,
        checked: false
      })

      this.newTask = this.initialNewTask()
    },

    setTask() {
      const { id, name, indexEditing, isEditing, checked } = this.newTask

      if(name) {
        if(isEditing) {
          this.setTaskEditing({ id, name, checked, indexEditing })
          return
        }

        this.setNewTask(name)
        return 
      }

      alert('Por favor, preencha o campo para inserir a sua tarefa.')
    },

    checkTask(index, task) {
      const newListTask = [...this.listTasks]
      newListTask.splice(index, 1, { ...task, checked: !task.checked })
      this.listTasks = newListTask
    },

    editTask(index, task) {
      this.newTask = {
        ...task,
        indexEditing: index,
        isEditing: true
      }
    }
  }
}
</script>

<style lang="scss">
.app {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 800px;
}

.card-task {
  width: 500px;
}

.card-task-input {
  width: 100%;
}

.task {
  display: flex;
}

.task-text {
  margin-left: 10px;
}

.texto-riscado {
  text-decoration: line-through;
}
</style>
