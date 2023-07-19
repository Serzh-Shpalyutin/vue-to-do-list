<script setup>
import { ref, onMounted } from 'vue';
import TaskInput from './components/TaskInput.vue';
import TaskCard from './components/TaskCard.vue';
import Cookies from 'js-cookie';

const tasks = ref([]);

// При монтировании компонента считываем данные из куки и обновляем список задач
onMounted(() => {
  const savedTasks = Cookies.get('tasks');
  if (savedTasks) {
    tasks.value = JSON.parse(savedTasks);
  }
});

const addTask = (task) => {
  tasks.value.push(task);
  Cookies.set('tasks', JSON.stringify(tasks.value));
};

const removeTask = (index) => {
  tasks.value.splice(index, 1);
  Cookies.set('tasks', JSON.stringify(tasks.value));
};
</script>

<template>
  <main class="main">
    <h1 class="main__title">Todo List</h1>
    <TaskInput @addTask="addTask" />

    <div v-if="tasks.length" class="tasks">
      <h2 class="tasks__title">Tasks</h2>

      <div class="tasks__list">
        <TaskCard v-for="(task, index) in tasks" :key="index" :title="task.title" :descr="task.descr"
          @onRemove="removeTask(index)" />
      </div>
    </div>
  </main>
</template>

<style scoped>
.main__title {
  margin-bottom: 50px;
  font-weight: 800;
}
</style>
