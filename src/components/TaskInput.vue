<script setup>
import { ref, defineEmits  } from 'vue';
import Cookies from 'js-cookie';

const emit = defineEmits('addTask')
const title = ref('');
const descr = ref('');

const addTask = () => {
  if (title.value.trim() === '' || descr.value.trim() === '') {
    alert('Please fill in both fields.');
    return;
  }

  emit('addTask', {
    title: title.value,
    descr: descr.value,
  });

  title.value = '';
  descr.value = '';
};

</script>

<template>
    <div class="task-input">
        <input v-model="title" class="task-input__input" type="text" placeholder="Title" />
        <input v-model="descr" type="text" class="task-input__input" placeholder="Add a new task" @keyup.enter="addTask">
    </div>
</template>

<style scoped>
.task-input {
    margin-bottom: 50px;
}

.task-input__input {
    width: 100%;
    padding: 2rem;
    border-radius: 1rem;
    border: .5rem solid #8338ec;
    font-family: 'JetBrains Mono', sans-serif;
    font-size: 2rem;
    transition: box-shadow .3s, transform .3s;
}

.task-input__input:not(:last-child) {
    margin-bottom: 1rem;
}

.task-input__input:focus {
    box-shadow: 5px 5px #00000080;
    transform: translateY(-5px);
    outline: none;
}
</style>