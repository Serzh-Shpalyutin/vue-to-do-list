<script setup>
import Cookies from 'js-cookie';
import { ref, defineProps, defineEmits, onMounted } from 'vue';

const { title, descr } = defineProps(['title', 'descr']);
const isDone = ref(false);
const emit = defineEmits();
const taskCardRef = ref(null);

// При монтировании компонента восстанавливаем статус выполнения
onMounted(() => {
    const savedIsDone = localStorage.getItem(`isDone_${title}`);
    if (savedIsDone !== null) {
        isDone.value = JSON.parse(savedIsDone);
    }
});

const onDone = () => {
    isDone.value = !isDone.value;
    localStorage.setItem(`isDone_${title}`, JSON.stringify(isDone.value));
};

const onRemove = () => {
    taskCardRef.value.classList.toggle('task-card__action-remove');
    setTimeout(() => {
        emit('onRemove');
    }, 250); // Задержка в 0.5 секунды (250 миллисекунд)
};
</script>

<template>
    <article class="task-card" :class="{ 'task-card__action-complete': isDone }" ref="taskCardRef">
        <div class="task-card__left">
            <h3 class="task-card__title">{{ title }}</h3>

            <p class="task-card__description">
                {{ descr }}
            </p>
        </div>

        <div class="task-card__actions">
            <button v-if="!isDone" @click="onDone" class="task-card__action task-card__action-complete">&#9989;</button>
            <button v-if="isDone" @click="onRemove" class="task-card__action task-card__action-remove">&#128683;</button>
        </div>
    </article>
</template>

<style scoped>
.task-card {
    padding: 0 0 0 2rem;
    border-radius: 1rem;
    border: .5rem solid #8338ec;
    transition: box-shadow .3s, transform .3s;

    display: flex;
    align-items: stretch;
    justify-content: space-between;
}

.task-card:not(:last-child) {
    margin-bottom: 2rem;
}

.task-card:hover {
    box-shadow: 5px 5px #00000080;
    transform: translateY(-5px);
}

.task-card:hover .task-card__actions {
    width: 7rem;
    opacity: 1;
}

.task-card__actions {
    width: 0rem;
    opacity: 0;
    background-color: #8338ec;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    transition: width .3s ease-in-out, opacity .2s ease-in-out;
}

.task-card__action {
    font-size: 3rem;
    transition: opacity .2s ease-in-out;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
    transform: translateX(.3rem);
}

.task-card__action:hover {
    opacity: .7;
}

.task-card__action:active {
    opacity: 1;
    transform: translateY(1px);
}

.task-card.task-card__action-complete {
    background-color: #03d00046;
}

.task-card.task-card__action-remove {
    background-color: #ff006e;
}
</style>