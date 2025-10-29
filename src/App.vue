<template>
    <main>
        <Nav 
            v-model:searchTerm="searchTerm"
            :toggleSubject="toggleSubject" 
            :toggleLocation="toggleLocation" 
            :toggleSort="toggleSort" 
            :toggleAvailability="toggleAvailability"
            :setPriceSort="setPriceSort"
            :setAvailabilitySort="setAvailabilitySort"
            :selectedSubjects="selectedSubjects" 
            :selectedLocations="selectedLocations" 
            :selectedAvailability="selectedAvailability"
            :sortOrder="sortOrder" 
            :toggleCart="toggleCart"
            :cart="cart"
        />
        <ClassBox v-if="!showCart" :classes="filteredClasses" @add-to-cart="addToCart" @remove-from-cart="removeFromCart" />
        <ShoppingCart v-else :cart="cart" @add-to-cart="addToCart" @remove-from-cart="removeFromCart" />
        <footer>&copy; Filip Domanski <span id="year">{{ year }}</span></footer>
    </main>
</template>

<script setup>
import { ref, computed, watch, onMounted } from 'vue';
import Nav from './components/Nav.vue';
import ClassBox from './components/ClassBox.vue';
import ShoppingCart from './components/ShoppingCart.vue';

const year = new Date().getFullYear();

// lists holding selected subjects/locations
const selectedSubjects = ref([]);
const selectedLocations = ref([]);
const selectedAvailability = ref(false);
const sortOrder = ref('price-asc');
const searchTerm = ref('');
const classes = ref([]);
const userId = getOrCreateUserId();

// Shopping Cart logic
const showCart = ref(false);
const toggleCart = () => {
    showCart.value = !showCart.value;
};

// cart state
const cart = ref([]);
const addToCart = async (classInfo) => {
    if (!cart.value.some(item => item.id === classInfo.id)) {
        cart.value.push(classInfo);
    }
    await saveCartToBackend();
};

const removeFromCart = async (classInfo) => {
    cart.value = cart.value.filter(item => item.id !== classInfo.id);
    await saveCartToBackend();
};

// Save randomgenerated userID in local storage for persistent cart logic
function getOrCreateUserId() {
  let userId = localStorage.getItem('userId');
  if (!userId) {
    userId = crypto.randomUUID();
    localStorage.setItem('userId', userId);
  }
  return userId;
}

// fetch saved cart if exists 
const fetchCart = async () => {
  const userId = localStorage.getItem('userId');
  const response = await fetch(`http://fs-cw-express-env-1.eba-xnwxzufd.eu-west-2.elasticbeanstalk.com/cart?userId=${userId}`);
  cart.value = await response.json();
};
onMounted(fetchCart);

// Save cart to backend
async function saveCartToBackend() {
    const userId = localStorage.getItem('userId');
    await fetch('http://fs-cw-express-env-1.eba-xnwxzufd.eu-west-2.elasticbeanstalk.com/cart', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ userId, items: cart.value })
    });
}

// Function to fetch classes from backend
const fetchClasses = async () => {
    const params = new URLSearchParams();
    if (searchTerm.value.trim()) params.append('q', searchTerm.value);
    if (selectedSubjects.value.length) params.append('subjects', selectedSubjects.value.join(','));
    if (selectedLocations.value.length) params.append('locations', selectedLocations.value.join(','));
    if (selectedAvailability.value) params.append('availability', 'true');
    params.append('sort', sortOrder.value);

    try {
        // const response = await fetch(`http://localhost:3000/search?${params}`);
        const response = await fetch(`http://fs-cw-express-env-1.eba-xnwxzufd.eu-west-2.elasticbeanstalk.com/search?${params}`);
        classes.value = await response.json();
    } catch (error) {
        console.error('Fetch error:', error);
    }
};

// Watch for changes and fetch
watch(searchTerm, fetchClasses);
watch(selectedSubjects, fetchClasses, { deep: true });
watch(selectedLocations, fetchClasses, { deep: true });
watch(selectedAvailability, fetchClasses);
watch(sortOrder, fetchClasses);

// Initial fetch
onMounted(fetchClasses);

const filteredClasses = computed(() => classes.value);

const toggleSubject = (subject) => {
    // If the subject is already selected, remove it from the list
    if (selectedSubjects.value.includes(subject)) {
        selectedSubjects.value = selectedSubjects.value.filter(s => s !== subject);
    // Otherwise, add it to the selected subjects
    } else {
        selectedSubjects.value.push(subject);
    }
};

const toggleLocation = (location) => {
    // If the location is already selected, remove it from the list
    if (selectedLocations.value.includes(location)) {
        selectedLocations.value = selectedLocations.value.filter(l => l !== location);
    // Otherwise, add it to the selected locations
    } else {
        selectedLocations.value.push(location);
    }
};

const toggleSort = () => {
    if (sortOrder.value === 'price-asc') sortOrder.value = 'price-desc';
    else if (sortOrder.value === 'price-desc') sortOrder.value = 'subject-asc';
    else if (sortOrder.value === 'subject-asc') sortOrder.value = 'subject-desc';
    else sortOrder.value = 'price-asc';
};

const toggleAvailability = () => {
    selectedAvailability.value = !selectedAvailability.value;
};

const setPriceSort = () => {
    sortOrder.value = 'price-asc';
};

const setAvailabilitySort = () => {
    if (sortOrder.value === 'availability-asc') {
        sortOrder.value = 'availability-desc';
    } else {
        sortOrder.value = 'availability-asc';
    }
};
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
