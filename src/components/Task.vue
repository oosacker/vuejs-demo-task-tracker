<template>
    <div @dblclick="$emit('toggle-reminder', task.id)" :class="[task.reminder ? 'reminder' : 'no-reminder', 'task']">
        <h3>
            {{ task.text }}
            <!-- <i @click="onDelete(task.id)" class='fas fa-times' ></i> -->
            <i @click="$emit('delete-task', task.id)" class='fas fa-times' ></i>
        </h3>
        <h5>{{ task.date }}</h5>
    </div>
</template>

<script>
    export default {
        name: 'Task',
        props: {
            task: Object,
        },  
        methods: {
            /* onDelete(id) {
                //console.log(input)
                this.$emit('delete-task', id) // emit the id up to the Tasks component
            } */
        }
    }
</script>

<style scoped>
    .task {
        margin: 5px;
        background-color: lightyellow;
        padding: 10px 20px;
        cursor: pointer;
    }

    .task.no-reminder {
        border-left: 5px solid transparent;
    }

    .task.reminder {
        border-left: 5px solid green;
    }

    .task h3 {
        display: flex;
        align-items: center;
        justify-content: space-between;
    }

    .fas.fa-times {
        color: red;
    }
</style>