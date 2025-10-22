<template>
    <section class="class-grid">
        <Card
            v-for="classItem in classes"
            :key="classItem.id"
            :class-info="classItem"
            @add-to-cart="$emit('add-to-cart', $event)"
            @remove-from-cart="$emit('remove-from-cart', $event)"
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

const emit = defineEmits(['add-to-cart', 'remove-from-cart']);
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
    margin-bottom: 2rem;
}
</style>