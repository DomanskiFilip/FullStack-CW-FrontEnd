<template>
    <main>
        <Nav />
        <footer>&copy; Filip Domanski <span id="year">{{ year }}</span></footer>
    </main>
</template>

<script setup>
    import { ref } from 'vue';
    import Nav from './components/Nav.vue';
    
    const year = new Date().getFullYear();
</script>


<style scoped>
main {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    background-color: var(--primary);
}

footer {
    width: 100%;
    height: 2rem;
    background-color: var(--secondary);
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    position: fixed;
    bottom: 0;
    left: 0;
    gap: 0.25rem;
}
</style>
