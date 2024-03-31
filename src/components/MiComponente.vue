<script setup>
import { ref, onMounted } from "vue"
import autoAnimate from "@formkit/auto-animate"

const dropdown = ref() // we need a DOM node
const show = ref(false)

onMounted(() => {
    autoAnimate(dropdown.value) // thats it!
})
</script>

<template>
    <div ref="dropdown" class="dropdown">
        <strong class="dropdown-label" @click="show = !show">
            Click me to open!
        </strong>
        <p class="dropdown-content" v-if="show">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Dicta, tenetur
            ad deleniti dolorem delectus numquam repudiandae repellendus aperiam vero. Quam possimus cum veniam natus
            sint modi consectetur. Saepe, libero mollitia!</p>
    </div>
</template>
<style>
    .dropdown {
        border: solid 2px;
        border-radius: 5px;
    }
</style>
