<template>
    <section class="class-grid">
        <Card
            v-for="classItem in classes"
            :key="classItem.id"
            :class-info="classItem"
        />
    </section>
</template>

<script setup>
import Card from './Card.vue';

const props = defineProps({
    classes: {
        type: Array,
        required: true
    }
});
</script>

<style scoped>
.class-grid {
    flex: 1;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(500px, 1fr));
    gap: 1rem;
    padding: 1rem;
    overflow-y: auto;
    box-sizing: border-box;
}
</style>