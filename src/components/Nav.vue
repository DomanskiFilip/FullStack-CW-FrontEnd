<template>
    <section>
        <h1>ClassShop</h1>
        <div class="center">
            <input type="text" id="search" placeholder="Search..." />
            <div class="tags-wrapper">
                <button id="tags" @click="showTags = !showTags">
                    <svg xmlns="http://www.w3.org/2000/svg" height="24px" viewBox="0 -960 960 960" width="24px" fill="#e3e3e3"><path d="m159-168-34-14q-31-13-41.5-45t3.5-63l72-156v278Zm160 88q-33 0-56.5-23.5T239-160v-240l106 294q3 7 6 13.5t8 12.5h-40Zm206-4q-32 12-62-3t-42-47L243-622q-12-32 2-62.5t46-41.5l302-110q32-12 62 3t42 47l178 488q12 32-2 62.5T827-194L525-84Zm-86-476q17 0 28.5-11.5T479-600q0-17-11.5-28.5T439-640q-17 0-28.5 11.5T399-600q0 17 11.5 28.5T439-560Zm58 400 302-110-178-490-302 110 178 490ZM319-650l302-110-302 110Z"/></svg>
                </button>
                <ul id="taglist" v-if="showTags">
                    <li @mouseenter="showSubjects = true" @mouseleave="showSubjects = false">
                        subject
                        <svg xmlns="http://www.w3.org/2000/svg" height="16px" viewBox="0 -960 960 960" width="16px" fill="#e3e3e3"><path d="m288-96-68-68 316-316-316-316 68-68 384 384L288-96Z" stroke="#e3e3e3" stroke-width="80"/></svg>
                        <ul class="sublist" v-if="showSubjects">
                            <li v-for="subject in subjects" :key="subject" @click="toggleSubject(subject)" :class="{ active: selectedSubjects.includes(subject) }">{{ subject }}</li>
                        </ul>
                    </li>
                    <li @mouseenter="showLocations = true" @mouseleave="showLocations = false">
                        location
                        <svg xmlns="http://www.w3.org/2000/svg" height="16px" viewBox="0 -960 960 960" width="16px" fill="#e3e3e3"><path d="m288-96-68-68 316-316-316-316 68-68 384 384L288-96Z" stroke="#e3e3e3" stroke-width="80"/></svg>
                        <ul class="sublist" v-if="showLocations">
                            <li v-for="location in locations" :key="location" @click="toggleLocation(location)" :class="{ active: selectedLocations.includes(location) }">{{ location }}</li>
                        </ul>
                    </li>
                    <li>price</li>
                    <li>availability</li>
                </ul>
            </div>
            <button id="sort" @click="toggleSort">
                <svg xmlns="http://www.w3.org/2000/svg" height="18px" viewBox="0 -960 960 960" width="18px" fill="#e3e3e3"><path d="M120-240v-80h240v80H120Zm0-200v-80h480v80H120Zm0-200v-80h720v80H120Z"/></svg>
            </button>
        </div>
        <button id="cart"><svg xmlns="http://www.w3.org/2000/svg" height="18px" viewBox="0 -960 960 960" width="18px" fill="#e3e3e3"><path d="M280-80q-33 0-56.5-23.5T200-160q0-33 23.5-56.5T280-240q33 0 56.5 23.5T360-160q0 33-23.5 56.5T280-80Zm400 0q-33 0-56.5-23.5T600-160q0-33 23.5-56.5T680-240q33 0 56.5 23.5T760-160q0 33-23.5 56.5T680-80ZM246-720l96 200h280l110-200H246Zm-38-80h590q23 0 35 20.5t1 41.5L692-482q-11 20-29.5 31T622-440H324l-44 80h480v80H280q-45 0-68-39.5t-2-78.5l54-98-144-304H40v-80h130l38 80Zm134 280h280-280Z"/></svg></button>
    </section>
</template>

<script setup>
import { ref } from 'vue';

const props = defineProps({
    toggleSubject: Function,
    toggleLocation: Function,
    toggleSort: Function,
    selectedSubjects: Array,
    selectedLocations: Array,
    sortOrder: String
});

const showTags = ref(false);
const showSubjects = ref(false);
const showLocations = ref(false);

const subjects = ref([
    'Mathematics',
    'Physics',
    'Chemistry',
    'Biology',
    'Computer Science',
    'English',
    'History',
    'Geography',
    'Art',
    'Music'
]);

const locations = ref([
    'London Hendon Campus',
    'London Central Campus',
    'Manchester Campus',
    'Edinburgh Campus',
    'Cambridge Campus',
    'Oxford Campus',
    'Warsaw Campus',
    'Berlin Campus',
    'Paris Campus',
    'Krakow Campus'
]);
</script>

<style scoped>
section {
    width: 99%;
    height: 3rem;
    background-color: var(--secondary);
    margin: 1rem 0.5em;
    border-radius: 0.5rem;
    position: relative;
    display: flex;
    align-items: center;
    justify-content: flex-start;
}

.center {
    width: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 0.5rem;
    padding-right: 4.5rem;
}

h1 {
    color: white;
    font-size: 2rem;
    font-weight: bold;
    font-family: Impact, Haettenschweiler, 'Arial Narrow Bold', sans-serif;
    position: absolute;
    left: 1rem; 
    margin: 0;
    text-shadow: var(--accent);
}

#search {
    height: 2rem;
    width: 25%;
    border-radius: 0.5rem;
    border: none;
    padding-left: 0.5rem;
    font-size: 1rem;
}

#search:focus {
    outline: 5px solid var(--action);
}

button {
    width: 2.5rem;
    height: 2.5rem;
    border-radius: 0.5rem;
    background-color: var(--action);
    color: var(--text);
    border: none;
    display: flex;
    justify-content: center;
    align-items: center;
}

button:hover {
    cursor: pointer;
    background-color: var(--action-hover);
}

button:active {
    background-color: var(--action-active);
}

#cart {
    position: absolute;
    right: 1rem;
}

.tags-wrapper {
    position: relative;
    display: flex;
    align-items: center;
}

#taglist {
    position: absolute;
    top: calc(100% + 0.5rem);
    left: 50%;
    transform: translateX(-50%);
    background-color: var(--surface);
    list-style: none;
    display: flex;
    flex-direction: column;
    z-index: 10;
}

#taglist > li {
    padding: 0.5rem;
    background-color: var(--action);
    color: var(--text);
    cursor: pointer;
    display: flex;
    align-items: center;
    position: relative;
}

#taglist > li svg {
    margin-left: auto;
    width: 12px;
    height: 12px;
}

#taglist > li:hover {
    background-color: var(--action-hover);
}

.sublist {
    position: absolute;
    left: 100%;
    top: 0;
    background-color: var(--surface);
    list-style: none;
    display: flex;
    flex-direction: column;
    min-width: 150px;
}

.sublist li {
    padding: 0.5rem;
    background-color: var(--action);
    color: var(--text);
    cursor: pointer;
}

.sublist li:hover {
    background-color: var(--action-hover);
}

.sublist li.active {
    background-color: var(--accent);
    color: var(--secondary);
}
</style>