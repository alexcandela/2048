<template>
    <div>
        <button @click="sum()">Click Me</button>
        <span>{{ count }}</span>
    </div>
</template>
<script>
export default {
    data() {
        return {
            count: 0
        }
    },
    methods: {
        sum(){
            this.count++;
        }
    },
}
</script>
<style lang="">
    
</style>