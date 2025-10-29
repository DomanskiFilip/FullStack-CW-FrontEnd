<template>
    <section  v-if="showCheckoutForm" class="checkout-form-overlay">
        <form @submit.prevent="submitCheckout">
            <h3>Checkout</h3>
            <label>
                Name:
                <input v-model="checkoutInfo.name" required />
            </label>
            <label>
                Email:
                <input v-model="checkoutInfo.email" type="email" required />
            </label>
            <label>
                Address:
                <input v-model="checkoutInfo.address" required />
            </label>
            <label>
                Credit Card Number:
                <input v-model="checkoutInfo.creditCardNumber" type="text" required />
            </label>
            <p>Total price: {{ totalPrice }}</p>
            <button type="submit">Confirm Order</button>
            <button type="button" @click="showCheckoutForm = false">Cancel</button>
        </form>
    </section>

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
        <button
            v-if="cart.length > 0 && !showCheckoutForm"
            @click="showCheckoutForm = true"
            id="checkout"
        >checkout</button>
        <button v-else-if="!showCheckoutForm" id="checkout" disabled>checkout</button>
    </section>
</template>

<script setup>
import { ref, computed } from 'vue';
import Card from './Card.vue';

const props = defineProps({
    cart: {
        type: Array,
        required: true
    }
});

const emit = defineEmits(['add-to-cart', 'remove-from-cart', 'checkout', 'clear-cart']);

const showCheckoutForm = ref(false);

const checkoutInfo = ref({
    name: '',
    email: '',
    address: '',
    creditCardNumber: ''
});

const totalPrice = computed(() =>
    props.cart.reduce((total, item) => total + (item.price || 0) * (item.quantity || 1), 0)
);

async function submitCheckout() {
    // Validate credit card number
    if (isNaN(Number(checkoutInfo.value.creditCardNumber))) {
        alert('Credit card number must be a number.');
        return;
    }

    // Save order to backend
    const userId = localStorage.getItem('userId');
    const orderRes = await fetch('http://fs-cw-express-env-1.eba-xnwxzufd.eu-west-2.elasticbeanstalk.com/order', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
            userId,
            userInfo: checkoutInfo.value
        })
    });
    const orderData = await orderRes.json();

    // Update lessons (decrement availablePlaces)
    for (const item of props.cart) {
        await fetch(`http://fs-cw-express-env-1.eba-xnwxzufd.eu-west-2.elasticbeanstalk.com/lesson/${item._id}`, {
            method: 'PUT',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({
                availablePlaces: item.availablePlaces - 1
            })
        });
    }

    // Clear cart in frontend
    emit('clear-cart');
    showCheckoutForm.value = false;

    // Clear cart in backend
    await fetch('http://fs-cw-express-env-1.eba-xnwxzufd.eu-west-2.elasticbeanstalk.com/cart', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ userId, items: [] })
    });
}
</script>

<style scoped>
.cart {
    position: relative;
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 1rem;
    background-color: var(--primary);
    color: var(--text);
    overflow-y: auto;
    z-index: 1;
}

.checkout-form-overlay {
    position: fixed;
    top: 10rem;
    left: 50%;
    transform: translateX(-50%);
    width: 60%;
    max-width: 90%;
    background: var(--primary);
    box-shadow: 0 0 20px rgba(0,0,0,0.15);
    border-radius: 0.75rem;
    z-index: 100;
    padding: 1.5rem;
    display: flex;
    flex-direction: column;
    align-items: stretch;
    box-shadow: 0 0 0 100vmax rgba(0,0,0,0.15);
    color: var(--text)
}

.checkout-form-overlay form {
    display: flex;
    flex-direction: column;
    gap: 1rem;
}

.checkout-form-overlay input {
    width: 100%;
    padding: 0.5rem;
    border-radius: 0.25rem;
    border: 1px solid var(--action);
}

.checkout-form-overlay button {
    margin-top: 0.5rem;
    padding: 0.5rem;
    border-radius: 0.25rem;
    border: none;
    background: var(--action);
    color: var(--text);
    font-weight: bold;
    cursor: pointer;
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
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 1rem;
    margin-top: 1rem;
    width: 100%;
}

#checkout-section {
    position: absolute;
    display: flex;
    flex-direction: column;
    background-color: var(--primary);
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    margin-top: 1rem;
    font-size: 1.25rem;
    font-weight: bold;
    z-index: 2;
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