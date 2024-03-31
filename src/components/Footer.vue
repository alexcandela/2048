<template>
    <footer class="footer">
        <a v-for="link in links" :key="link.name" href={{link.ruta}}>{{ link.name }}</a>
    </footer>
</template>
<script>
export default {
    data() {
        return {
            links: [
                { name: 'Sobre Nosotros', ruta: '#' },
                { name: 'Avisos Legales', ruta: '#' },
                { name: 'Contactanos', ruta: '#' },
            ]
        }
    },
}
</script>
<style>
</style>