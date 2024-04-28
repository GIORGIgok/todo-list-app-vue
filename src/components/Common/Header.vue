<script setup>
import { ref, onMounted } from 'vue'

const isDarkMode = ref(false);

const toggleDarkMode = () => {
    isDarkMode.value = !isDarkMode.value;
    document.querySelector('html').classList.toggle('dark', isDarkMode.value);
    saveDarkMode();
}

const saveDarkMode = () => {
    localStorage.setItem('darkMode', JSON.stringify(isDarkMode.value));
}

onMounted(() => {
    const storedDarkMode = localStorage.getItem('darkMode');
    if (storedDarkMode) {
        isDarkMode.value = JSON.parse(storedDarkMode);
        document.querySelector('html').classList.toggle('dark', isDarkMode.value);
    }
})


</script>

<template>
    <header
        class="w-full px-4 md:px-20 h-[80px] fixed top-0 bg-gradient-to-r from-gray-900 via-gray-800 to-gray-800 flex justify-between items-center"
    >
        <div>
            <h1 class="text-lg md:text-4xl bg-gradient-to-r from-green-600 via-green-400 to-green-600 text-transparent bg-clip-text font-bold">Vue3</h1>
        </div>
        <nav>
            <ul class="flex justify-between gap-6">
                <li class="font-bold flex items-center">
                    <RouterLink to="/" active-class="text-indigo-300 hover:text-gray-500 transition-colors duration-300">To-Do List</RouterLink>
                </li>
                <div class="w-[1px] h-[30px] bg-gray-400"></div>
                <li @click="toggleDarkMode" class="text-gray-200 hover:text-orange-600 font-bold cursor-pointer flex items-center" :class="{ 'text-orange-600': isDarkMode }">
                    Dark Mode
                </li>
                <div class="w-[1px] h-[30px] bg-gray-400"></div>
            </ul>
        </nav>
    </header>
</template>

<style scoped>
header {
    border-bottom: 1.5px solid #424242;
    box-shadow: 0 0 10px #00213b;
}
nav ul li {
    transition: all 0.3s ease;
    font-size: 20px;
}
@media screen and (max-width: 576px) {
    nav ul {
        gap: 8px;
    }
    nav ul li {
        font-size: 16px;
    }
}

</style>

<!-- GGoqadze -->