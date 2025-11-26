<template>
    <article class="card">
        <header>
            <h2>{{ classInfo.subject }}</h2>
        </header>
        <p class="location">{{ classInfo.location }}</p>
        <p class="availability">{{ classInfo.availablePlaces }} places available</p>
        <img :src="classInfo.image || fallbackImage" :alt="`${classInfo.subject} class image`" />
        <section>
            <p class="price">£{{ classInfo.price }}</p>
            <button v-if="classInfo.availablePlaces > 0 && !isInCart(classInfo)" @click="addToCart(classInfo)">Add to Cart</button>
            <button v-else-if="isInCart(classInfo)" @click="removeFromCart(classInfo)">Remove from Cart</button>
            <button v-else disabled>Full</button>
        </section>
    </article>
</template>

<script setup>
const fallbackImage = 'https://placehold.co/600x400?text=Class';

const props = defineProps({
    classInfo: {
        type: Object,
        required: true
    }
});

const emit = defineEmits(['add-to-cart', 'remove-from-cart']);

const addToCart = (classInfo) => {
    emit('add-to-cart', classInfo);
};
const removeFromCart = (classInfo) => {
    emit('remove-from-cart', classInfo);
};

const isInCart = (classInfo) => {
    return props.classInfo.inCart === true;
};
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

.availability {
    font-size: 0.9rem;
    color: var(--muted);
}

.price {
    font-size: 1.1rem;
    font-weight: 600;
}

section {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: auto;
}

button {
    background-color: var(--action);
    color: white;
    border: none;
    border-radius: 0.5rem;
    padding: 0.5rem 1rem;
    cursor: pointer;
    font-size: 1rem;
    transition: background-color 0.3s ease;
}

button:hover {
    background-color: var(--action-hover);
}

button:active {
    background-color: var(--action-active);
}

button:disabled {
    background-color: var(--muted);
    opacity: 0.5;
    cursor: not-allowed;
}

img {
    width: 100%;
    height: 140px;
    object-fit: cover;
    border-radius: 0.5rem;
    background-color: var(--surface);
}
</style>