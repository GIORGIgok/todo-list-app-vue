<script setup>
import { ref, onMounted } from 'vue'

const newTodo = ref('');
const todos = ref([]);

const addTodo = () => {
    if (newTodo.value.trim() !== "") {
        todos.value.push({ text: newTodo.value, done: false });
        newTodo.value = "";
        saveTodos();
    }
};

const markTodoAsDone = (index) => {
    todos.value[index].done = !todos.value[index].done;
    saveTodos();
}

const removeTodo = (index) => {
    todos.value = todos.value.filter((_, i) => i !== index);
    saveTodos();
}

const saveTodos = () => {
    localStorage.setItem('todos', JSON.stringify(todos.value));
}

const loadTodos = () => {
    const storedTodos = localStorage.getItem('todos');
    if (storedTodos) {
        todos.value = JSON.parse(storedTodos);
    }
}

onMounted(() => {
    loadTodos();
});

const removeDoneTodos = () => {
    todos.value = todos.value.filter(todo => !todo.done);
}
</script>

<template>
    <form
    @submit.prevent="addTodo"
        class="sm:w-3/5 w-full mx-auto mt-4 shadow-md rounded px-2 md:px-8 py-6 flex flex-col items-center dark:bg-gradient-to-br dark:from-slate-600 dark:via-cyan-800 dark:to-slate-500 bg-gradient-to-br from-gray-300 via-indigo-200 to-gray-400 min-h-[380px]">
        <div class="flex flex-col items-center sm:block">
            <label for="new-todo">
                <input v-model="newTodo" @keyup.enter="addTodo" id="new-todo" type="text" name="new-todo"
                    class="shadow appearance-none rounded-lg sm:rounded-r w-full-[-24] py-2 px-3 text-gray-700 border-green-300 focus:outline-none border-2 focus-within:border-green-400 focus-within:text-emerald-800" placeholder="Enter a new task..."/>
            </label>
            <button type="submit"
                class="text-gray-100 shadow bg-green-600 hover:bg-green-400 focus:shadow-outline font-bold py-2 px-4 rounded-lg sm:rounded-l w-24 focus:ring-2 focus:ring-green-500 border-2 border-green-600 hover:text-slate-800 transition-all duration-500">
                Add
            </button>
        </div>

            <TransitionGroup name="fade" tag="ul" class="w-full my-8 flex flex-col items-center gap-2" :class="{ 'hidden': todos.length === 0}">
            <li v-for="(todo, index) in todos" :key="index" :class="{ 'line-through': todo.done }"
                class="min-w-full space-x-1 border-2 border-indigo-300 rounded-lg bg-white hover:bg-slate-200 px-4 py-2 font-bold overflow-hidden">
                <svg v-on:click="removeTodo(index)" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                        stroke="red" class="w-6 h-6 cursor-pointer float-left mr-2">
                        <path stroke-linecap="round" stroke-linejoin="round"
                            d="m9.75 9.75 4.5 4.5m0-4.5-4.5 4.5M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" />
                    </svg>
                <svg @click="markTodoAsDone(index)" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5"
                        stroke="green" class="w-6 h-6 cursor-pointer">
                        <path stroke-linecap="round" stroke-linejoin="round"
                            d="M9 12.75 11.25 15 15 9.75M21 12a9 9 0 1 1-18 0 9 9 0 0 1 18 0Z" />
                    </svg>
                    <div class="w-full h-[1px] bg-blue-400 my-2 float-left mx-auto"></div>
                <span v-if="todo">{{ todo.text }}</span>
            </li>
        </TransitionGroup>
        <Transition name="fade">
            <img v-if="!todos.length" src="../../assets/empty-todo.png" alt="empty todo" class="text-center my-4 
            opacity-30" />
        </Transition>


        <button 
        v-if="todos.some(todo => todo.done)"
        @click="removeDoneTodos"
        type="button"
            class="shadow bg-red-400 hover:bg-red-300 focus:shadow-outline font-bold py-2 px-4 rounded transition-colors duration-700">
            Clear completed
        </button>
    </form>
</template>

<style scoped>
/* transitions */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>

<!-- GGoqadze -->
