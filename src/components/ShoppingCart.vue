<template>
    <section class="cart">
        <section class="cart-header">
            <h2>Shopping Cart</h2>
            <p>Total items: {{ cart.length }}</p>
        </section>
        <section v-if="cart.length === 0">
            <p>Your cart is empty.</p>
        </section>
        <section v-else class="cart-items">
            <Card
                v-for="item in cart"
                :key="item.id"
                :class-info="{ ...item, inCart: true }"
                @add-to-cart="$emit('add-to-cart', $event)"
                @remove-from-cart="$emit('remove-from-cart', $event)"
            />
        </section>
        <button v-if="cart.length > 0" @click="$emit('checkout')" id="checkout">checkout</button>
        <button v-else id="checkout" disabled>checkout</button>
    </section>
</template>

<script setup>
import Card from './Card.vue';

const props = defineProps({
    cart: {
        type: Array,
        required: true
    }
});

const emit = defineEmits(['add-to-cart', 'remove-from-cart', 'checkout']);
</script>
    background-color: var(--action);
<style scoped>
.cart {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 1rem;
    background-color: var(--primary);
    color: var(--text);
    overflow-y: auto;
}

.cart-header {
    width: 100%;
    border-bottom: 1px solid var(--action);
    padding-bottom: 0.5rem;
    margin-bottom: 1rem;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.cart-items {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(500px, 1fr));
    gap: 1rem;
    margin-top: 1rem;
}

#checkout {
    margin-top: auto;
    margin-bottom: 3rem;
    width: 30%;
    padding: 0.75rem;
    background-color: var(--action);
    color: var(--text);
    border: none;
    border-radius: 0.5rem;
    font-size: 1rem;
    font-weight: bold;
}

#checkout:hover {
    background-color: var(--action-hover);
    cursor: pointer;
}

#checkout:active {
    background-color: var(--action-active);
}

#checkout:disabled {
    background-color: var(--muted);
    opacity: 0.5;
    cursor: not-allowed;
    margin-top: auto;
    margin-bottom: 3rem;
    width: 30%;
    padding: 0.75rem;
    color: var(--text);
    border: none;
    border-radius: 0.5rem;
    font-size: 1rem;
    font-weight: bold;
}
</style>