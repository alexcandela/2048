<template>
    <footer class="footer">
        <a v-for="link in links" :key="link.name" :href="link.ruta" target="_blank"><i :class="link.iconClass"></i>{{ link.name }}</a>
    </footer>
</template>
<script>
export default {
    data() {
        return {
            links: [
                { iconClass: 'fa-brands fa-github', name: 'GitHub', ruta: 'https://github.com/alexcandela' },
                { iconClass: 'fa-solid fa-desktop', name: 'Portfolio', ruta: 'https://alexcandela.github.io/' },
                { iconClass: 'fa-brands fa-linkedin', name: 'Linkedin', ruta: 'https://www.linkedin.com/in/alex-candela-b31956228/' },
            ]
        }
    },
}
</script>
<style>
</style>