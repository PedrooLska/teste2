<template>
  <div class="app">
    <div class="card-task">
      <input class="card-task-input" type="text" v-model="newTask" @keypress.enter="setNewTask">

      <div class="task" v-for="(task, index) in listTasks" :key="task.id">
        <input class="form-check-input" type="checkbox" @click="checkTask(index, task)">

        <div class="task-text" :class="{ 'texto-riscado': task.checked }">
          {{ task.name }}
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
      newTask: '',
      listTasks: []
    }
  },

  methods: {
    setNewTask() {
      const newId = this.listTasks.length === 0 ? 0 : [...this.listTasks].pop().id + 1

      this.listTasks.push({
        id: newId,
        name: this.newTask,
        checked: false
      })
    },

    checkTask(index, task) {
      const newListTask = [...this.listTasks]
      newListTask.splice(index, 1, { ...task, checked: !task.checked })
      this.listTasks = newListTask
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
