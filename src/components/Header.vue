<template>
    <header>
        <h1>{{ title }}</h1>
        <Button 
            :text="showAddTask ? 'Hide' : 'Add Task'" 
            :color="showAddTask ? 'Red' : 'Green'" 
            @btn-click="$emit('toggle-add-task')"
        />
    </header>
</template>

<script>
    import Button from './Button'

    export default {
        name: 'Header',
        components: {
            Button
        },
        props: {
            title: {
                type: String,
                default: 'HELLOOOOO',
            },
            showAddTask: Boolean
        },
        methods: {
            
        }
    }
</script>

<style scoped>
    header {
        display: flex;
        align-items: center;
        justify-content: space-between;
        margin-bottom: 20px;
    }
</style>