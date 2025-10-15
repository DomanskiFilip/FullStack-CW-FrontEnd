<template>
    <article class="card">
        <header>
            <h2>{{ classInfo.subject }}</h2>
        </header>
        <p class="location">{{ classInfo.location }}</p>
        <p class="price">£{{ classInfo.price }}</p>
    </article>
</template>

<script setup>
const props = defineProps({
    classInfo: {
        type: Object,
        required: true
    }
});
</script>

<style scoped>
.card {
    background-color: var(--secondary);
    border: 1px solid var(--action);
    border-radius: 0.75rem;
    padding: 1rem;
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
    color: var(--text);
    box-shadow: 0 6px 18px rgba(0, 0, 0, 0.25);
    height: 250px;
}

header h2 {
    font-size: 1.25rem;
    margin: 0;
}

.location {
    font-size: 0.95rem;
    color: var(--accent);
}

.price {
    font-size: 1.1rem;
    font-weight: 600;
    margin-top: auto;
}
</style>