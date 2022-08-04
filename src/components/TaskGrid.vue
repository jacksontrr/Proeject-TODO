<template>
    <div class="task-grid">
        <template v-if="tasks.length">
            <Task 
            v-for="(task, i) in tasks" 
            :key="task.name" 
            :task="task" 
            @taskDeleted="$emit('taskDeleted',i)" 
            @taskStateChanged="$emit('taskStateChanged', i)" />
            <!-- <Task v-for="task in tasks" :key="task.name" :task="task" @taskDeleted="$emit('taskDeleted', $event)" /> -->
        </template>
        <p v-else class="no-task"> There are no tasks =D</p>
    </div>
</template>

<script>

import Task from './Task.vue'

export default {
    components: {
        Task
    },
    props: {
        tasks: { type: Array, required: true }
    }
}
</script>

<style scoped>
.task-grid {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
}

.task-grid .task {
    margin: 10px;
}

.no-task {
    color: #AAA;
    font-size: 1.7rem;
}
</style>