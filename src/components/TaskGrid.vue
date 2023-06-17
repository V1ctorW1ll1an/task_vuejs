<template>
    <div class="task-grid">
        <template v-if="tasks.length">
            <Task
                v-for="(task, id) of tasks"
                :key="task.name"
                :task="task"
                @taskDeleted="$emit('taskDeleted', id)"
                @taskStateChanged="$emit('taskStateChanged', id)"
            >
            </Task>
        </template>
        <p v-else class="no-task">Sua vida está em dia :)</p>
    </div>
</template>

<script>
import Task from "./Task";
export default {
    components: {
        Task
    },
    props: {
        tasks: {
            type: Array,
            require: true
        }
    }
};
</script>

<style>
.task-grid {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
}

/* o .task é do outro componente task */
.task-grid > .task {
    margin: 10px;
}

.no-task {
    color: #aaa;
    font-size: 1.7rem;
}
</style>
