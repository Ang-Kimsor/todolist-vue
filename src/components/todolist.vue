<script setup>
import { ref } from 'vue';
const input = ref("")
const getItem = localStorage.getItem('tasks') ? JSON.parse(localStorage.getItem('tasks')) : []
const setItem = (item) => { localStorage.setItem('tasks', JSON.stringify(item)) }
const tasks = ref(getItem)

const addtask = () => {
    if (input.value.trim() !== '') {
        tasks.value.push({
            name: input.value,
            done: false
        })
        setItem(tasks.value)
    } else {
        alert("Please input valid value")
    }
    input.value = ""
}

const removetask = (i) => {
    tasks.value.splice(i, 1)
    setItem(tasks.value)
}

const markasdone = (i) => {
    const target = tasks.value.find((_, index) => index === i)
    target.done = !target.done
    setItem(tasks.value)
}

const clearAll = () => {
    tasks.value = []
    setItem(tasks.value)
}
</script>

<template>
    <main class="w-full h-screen bg-gradient-to-br from-[#4b79a1] to-[#283e51] flex items-center justify-center">

        <!-- CARD -->
        <div
            class="w-[95%] max-w-[480px] bg-white/20 backdrop-blur-xl border border-white/30 shadow-xl rounded-2xl p-6 flex flex-col">

            <!-- TITLE -->
            <h1 class="text-3xl text-white font-bold tracking-wide text-center mb-5">To Do List</h1>

            <!-- FORM -->
            <form @submit.prevent class="flex items-center gap-3 mb-5">
                <input type="text"
                    class="w-full px-4 py-2 rounded-lg bg-white/80 text-gray-900 placeholder-gray-500 outline-none"
                    placeholder="Enter your task here" v-model="input">

                <button @click="addtask"
                    class="px-4 py-2 bg-green-600 text-white font-semibold rounded-lg hover:bg-green-700 active:scale-95 transition">
                    Add
                </button>
            </form>

            <!-- EMPTY -->
            <p v-if="tasks.length === 0" class="text-white/80 text-center py-10 text-lg">
                No tasks added yet.
            </p>

            <!-- LIST -->
            <div v-else class="flex flex-col gap-2 overflow-y-auto pr-1" style="max-height: 290px;">

                <div v-for="v, index in tasks"
                    class="w-full bg-white/30 border border-white/30 rounded-xl px-4 py-3 flex justify-between items-center">
                    <p :class="[
                        'font-medium tracking-wide',
                        v.done ? 'line-through text-white/60' : 'text-white'
                    ]">
                        {{ v.name }}
                    </p>

                    <div class="flex items-center gap-3">
                        <input type="checkbox" :checked="v.done" @click="markasdone(index)"
                            class="w-4 h-4 cursor-pointer">

                        <button @click="removetask(index)"
                            class="px-3 py-1 bg-red-500 rounded-lg text-white text-sm hover:bg-red-600 active:scale-95">
                            Remove
                        </button>
                    </div>
                </div>

            </div>

            <!-- CLEAR BTN -->
            <button @click="clearAll"
                class="w-full mt-5 py-3 bg-red-600 text-white font-semibold rounded-lg hover:bg-red-700 active:scale-95">
                Clear All
            </button>

        </div>

    </main>
</template>
