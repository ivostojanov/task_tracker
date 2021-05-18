<template>
    <header>
        <h1>{{title}}</h1>     
        <Button v-show="homePage" @btn-click="$emit('toggle-add-task-form')" :text="showAddTaskForm?'Close':'Add Task'" :color="showAddTaskForm?'red':'green'" />                            
    </header>
</template>

<script>
    import Button from './Button'

    export default {
        name: 'Header',
        props: {
            title: {
                type: String,
                default: 'Hello World',
            },
            showAddTaskForm: Boolean, 
        },
        components: {
            Button
        },
        methods: {

        },
        computed: {
            homePage(){
                if(this.$route.path == '/'){
                    return true;
                }
                return false;
            }
        },
        emits: ['toggle-add-task-form']
    }
</script>

<style scoped>
    header{
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
</style>
