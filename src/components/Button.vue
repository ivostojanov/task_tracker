<template>
    <button class="btn" v-bind:style="{background:color}" @click="onClick()">{{text}}</button>
</template>

<script>
export default{
    name: 'Button',
    props: {
        text: {
            type: String,
            default: "Button Text"
        },
        color: {
            type: String,
            default: "black"
        }
    },
    methods: {
        log: function(message){
            console.log(message);
        },
        onClick: function(){
            this.$emit('btn-click')
        }
    }
}
</script>

<style scoped>

</style>
