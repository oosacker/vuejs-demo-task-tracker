<template>
    <header>
        <h1>{{ title }}</h1>
        <Button 
            text='Add Task' 
            color='green' 
            @toggle-add-task="$emit('toggle-add-task')"
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
                default: 'HELLOOOOO'
            }
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