<template>
    <div class='task-container' :key='task.id' v-for='task in tasks'>
        <Task :task=task />
    </div>
</template>

<script>
    import Task from './Task'

    export default {
        name: 'Tasks',
        props: {
            tasks: Array,
        },
        components: {
            Task
        }
    }
</script>

<style scoped>
    .task-container {
        margin-bottom: 20px;
        background-color: lightgrey;
        padding: 10px;
    }
</style>