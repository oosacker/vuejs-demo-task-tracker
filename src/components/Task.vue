<template>
    <h3>
        {{ task.text }}
        <i class="fas fa-times" ></i>
    </h3>
    <h5>{{ task.date }}</h5>
    
</template>

<script>
    export default {
        name: 'Task',
        props: {
            task: Object,
        }
    }
</script>

<style scoped>
    .fas {
        color: blue;
        float: right;
    }
</style>