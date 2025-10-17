<template>
    <main>
        <Nav 
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
        />
        <ClassBox v-if="!showCart" :classes="filteredClasses" />
        <ShoppingCart v-else />
        <footer>&copy; Filip Domanski <span id="year">{{ year }}</span></footer>
    </main>
</template>

<script setup>
import { ref, computed } from 'vue';
import Nav from './components/Nav.vue';
import ClassBox from './components/ClassBox.vue';
import ShoppingCart from './components/ShoppingCart.vue';

const year = new Date().getFullYear();

// lists holding selected subjects/locations
const selectedSubjects = ref([]);
const selectedLocations = ref([]);
const selectedAvailability = ref(false);

const sortOrder = ref('price-asc');

// Shopping Cart logic
const showCart = ref(false);
const toggleCart = () => {
    showCart.value = !showCart.value;
};

const classes = ref([
    { id: 1, subject: 'Mathematics', location: 'London Hendon Campus', price: 120, availablePlaces: 5 },
    { id: 2, subject: 'Mathematics', location: 'Oxford Campus', price: 128, availablePlaces: 10 },
    { id: 3, subject: 'Mathematics', location: 'Cambridge Campus', price: 135, availablePlaces: 0 },
    { id: 4, subject: 'Physics', location: 'London Hendon Campus', price: 140, availablePlaces: 7 },
    { id: 5, subject: 'Physics', location: 'Manchester Campus', price: 148, availablePlaces: 3 },
    { id: 6, subject: 'Physics', location: 'Edinburgh Campus', price: 155, availablePlaces: 9 },
    { id: 7, subject: 'Chemistry', location: 'London Central Campus', price: 132, availablePlaces: 1 },
    { id: 8, subject: 'Chemistry', location: 'Berlin Campus', price: 138, availablePlaces: 8 },
    { id: 9, subject: 'Chemistry', location: 'Paris Campus', price: 144, availablePlaces: 4 },
    { id: 10, subject: 'Biology', location: 'Oxford Campus', price: 125, availablePlaces: 6 },
    { id: 11, subject: 'Biology', location: 'Cambridge Campus', price: 131, availablePlaces: 2 },
    { id: 12, subject: 'Biology', location: 'Warsaw Campus', price: 137, availablePlaces: 10 },
    { id: 13, subject: 'Computer Science', location: 'London Central Campus', price: 160, availablePlaces: 0 },
    { id: 14, subject: 'Computer Science', location: 'Berlin Campus', price: 168, availablePlaces: 5 },
    { id: 15, subject: 'Computer Science', location: 'Krakow Campus', price: 176, availablePlaces: 7 },
    { id: 16, subject: 'English', location: 'Edinburgh Campus', price: 102, availablePlaces: 9 },
    { id: 17, subject: 'English', location: 'Paris Campus', price: 108, availablePlaces: 3 },
    { id: 18, subject: 'English', location: 'London Hendon Campus', price: 112, availablePlaces: 8 },
    { id: 19, subject: 'History', location: 'Manchester Campus', price: 118, availablePlaces: 4 },
    { id: 20, subject: 'History', location: 'Oxford Campus', price: 124, availablePlaces: 1 },
    { id: 21, subject: 'History', location: 'Cambridge Campus', price: 130, availablePlaces: 6 },
    { id: 22, subject: 'Geography', location: 'Manchester Campus', price: 115, availablePlaces: 10 },
    { id: 23, subject: 'Geography', location: 'Berlin Campus', price: 122, availablePlaces: 2 },
    { id: 24, subject: 'Geography', location: 'Paris Campus', price: 128, availablePlaces: 5 },
    { id: 25, subject: 'Art', location: 'Warsaw Campus', price: 96, availablePlaces: 7 },
    { id: 26, subject: 'Art', location: 'Paris Campus', price: 102, availablePlaces: 0 },
    { id: 27, subject: 'Art', location: 'London Central Campus', price: 108, availablePlaces: 9 },
    { id: 28, subject: 'Music', location: 'Cambridge Campus', price: 115, availablePlaces: 3 },
    { id: 29, subject: 'Music', location: 'Krakow Campus', price: 122, availablePlaces: 8 },
    { id: 30, subject: 'Music', location: 'Manchester Campus', price: 128, availablePlaces: 4 }
]);

const filteredClasses = computed(() => {
    // Start with the full list of classes
    let filtered = classes.value;
    // Filter by selected subjects if any are chosen
    if (selectedSubjects.value.length > 0) {
        filtered = filtered.filter(c => selectedSubjects.value.includes(c.subject));
    }
    // Filter by selected locations if any are chosen
    if (selectedLocations.value.length > 0) {
        filtered = filtered.filter(c => selectedLocations.value.includes(c.location));
    }
    // Filter by availability if selected
    if (selectedAvailability.value) {
        filtered = filtered.filter(c => c.availablePlaces > 0);
    }
    // Sort based on sortOrder
    if (sortOrder.value === 'price-asc') {
        filtered = [...filtered].sort((a, b) => a.price - b.price);
    } else if (sortOrder.value === 'price-desc') {
        filtered = [...filtered].sort((a, b) => b.price - a.price);
    } else if (sortOrder.value === 'subject-asc') {
        filtered = [...filtered].sort((a, b) => a.subject.localeCompare(b.subject));
    } else if (sortOrder.value === 'subject-desc') {
        filtered = [...filtered].sort((a, b) => b.subject.localeCompare(a.subject));
    } else if (sortOrder.value === 'availability-asc') {
        filtered = [...filtered].sort((a, b) => a.availablePlaces - b.availablePlaces);
    } else if (sortOrder.value === 'availability-desc') {
        filtered = [...filtered].sort((a, b) => b.availablePlaces - a.availablePlaces);
    }
    return filtered;
});

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
