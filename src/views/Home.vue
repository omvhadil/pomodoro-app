<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import { onMounted, ref, watch } from 'vue'

const addTask = ref(false)
const fitur = ref('working')
const second = ref(0)
const minute = ref(25)
const button = ref(true)

const start = () => {
  let timeOut = setInterval(() => {
    second.value--
  }, 1000)
  button.value = false
  return timeOut
}

watch(second, () => {
  if (second.value === -1) {
    second.value = 59
    minute.value--
  }
})

watch(minute, () => {
  if (minute.value === -1) {
    minute.value = 0
    second.value = 0
  }
})

const formatTime = (time) => {
  if (time < 10) {
    return `0${time}`
  }
  return time
}

const formatTheme = (theme) => {
  switch (theme) {
    case 'working':
      return 'bg-gradient-to-t from-emerald-600 to-emerald-400'
    case 'shortB':
      return 'bg-gradient-to-t from-amber-600 to-amber-400'
    case 'longB':
      return 'bg-gradient-to-t from-red-600 to-red-400'
    default:
      return 'bg-gradient-to-t from-emerald-600 to-emerald-400'
  }
}

const formatTheme2 = (theme) => {
  switch (theme) {
    case 'working':
      return 'border-emerald-500'
    case 'shortB':
      return 'border-amber-500'
    case 'longB':
      return 'border-red-500'
    default:
      return 'border-emerald-500'
  }
}

const formatTitle = (title) => {
  switch (title) {
    case 'working':
      return 'Work Session'
    case 'shortB':
      return 'Short Break'
    case 'longB':
      return 'Long Break'
    default:
      return 'Work Session'
  }
}

onMounted(() => {})
</script>

<template>
  <div class="w-full h-[40%] absolute" :class="formatTheme(fitur)"></div>
  <div class="w-full absolute top-[30%] lg:top-[17%]">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1440 320">
      <path
        fill="#ffffff"
        fill-opacity="1"
        d="M0,128L30,138.7C60,149,120,171,180,186.7C240,203,300,213,360,186.7C420,160,480,96,540,96C600,96,660,160,720,170.7C780,181,840,139,900,122.7C960,107,1020,117,1080,133.3C1140,149,1200,171,1260,160C1320,149,1380,107,1410,85.3L1440,64L1440,320L1410,320C1380,320,1320,320,1260,320C1200,320,1140,320,1080,320C1020,320,960,320,900,320C840,320,780,320,720,320C660,320,600,320,540,320C480,320,420,320,360,320C300,320,240,320,180,320C120,320,60,320,30,320L0,320Z"
      ></path>
    </svg>
  </div>
  <div class="mw-[400px] h-[100vh] mx-auto relative">
    <!-- header -->
    <header class="header w-full">
      <div class="container h-14 flex items-center justify-between">
        <a href="#" class="font-bold text-white">Pomodoro</a>
        <div class="flex gap-5">
          <button class="text-white text-lg"><i class="ri-moon-line"></i></button>
          <button class="text-white text-lg"><i class="ri-settings-3-line"></i></button>
        </div>
      </div>
    </header>
    <div class="flex justify-center gap-4 mt-8 text-white">
      <button @click="fitur = 'working'" :class="fitur === 'working' ? 'font-bold' : ''">
        Working
      </button>
      <button @click="fitur = 'shortB'" :class="fitur === 'shortB' ? 'font-bold' : ''">
        Short Break
      </button>
      <button @click="fitur = 'longB'" :class="fitur === 'longB' ? 'font-bold' : ''">
        Long Break
      </button>
    </div>
    <!-- clock -->
    <section>
      <div
        class="w-[250px] h-[250px] bg-white rounded-full mx-auto shadow-md mt-8 flex items-center justify-center"
      >
        <div
          class="w-[220px] h-[220px] bg-white rounded-full border-8 flex items-center justify-center"
          :class="formatTheme2(fitur)"
        >
          <div v-if="fitur === 'working'" class="text-center leading-10">
            <p class="text-slate-400">Work Session</p>
            <h1 class="text-5xl font-semibold">
              {{ formatTime(minute) }} : {{ formatTime(second) }}
            </h1>
            <p class="text-slate-400">Minute - Second</p>
          </div>
          <div v-if="fitur === 'shortB'" class="text-center leading-10">
            <p class="text-slate-400">Short Break</p>
            <h1 class="text-5xl font-semibold">10:00</h1>
            <p class="text-slate-400">Minute - Second</p>
          </div>
          <div v-if="fitur === 'longB'" class="text-center leading-10">
            <p class="text-slate-400">Long Break</p>
            <h1 class="text-5xl font-semibold">15:00</h1>
            <p class="text-slate-400">Minute - Second</p>
          </div>
        </div>
      </div>
    </section>
    <section class="container mt-4">
      <div class="text-center">
        <h6>Time {{ formatTitle(fitur) }}</h6>
      </div>
      <div class="mt-6 flex justify-center">
        <button
          v-if="button"
          class="text-white py-2 px-6 rounded-lg"
          :class="formatTheme(fitur)"
          @click="start()"
        >
          START
        </button>
        <button v-if="!button" class="text-white py-2 px-6 rounded-lg" :class="formatTheme(fitur)">
          STOP
        </button>
      </div>
      <div class="shadow-lg rounded-lg p-4 mt-4">
        <div class="flex justify-between pb-2">
          <span>Tasks</span>
          <button><i class="ri-more-2-fill"></i></button>
        </div>
        <hr />
        <div class="mt-2">
          <!-- list task -->
          <div class="flex justify-between bg-slate-200 py-3 px-2 rounded-md">
            <span class="font-semibold text-slate-600">Excemple Task One</span>
            <div class="flex text-slate-500">
              <button class="px-2" title="Edit"><i class="ri-edit-2-line"></i></button>
              <button class="px-2" title="Hapus"><i class="ri-delete-bin-6-line"></i></button>
            </div>
          </div>
        </div>
        <!-- card add task -->
        <div v-if="addTask" class="border w-full mt-2 rounded-lg p-3">
          <input
            type="text"
            placeholder="What are you working on?"
            class="focus:outline-none w-full"
          />
          <div class="flex gap-2 mt-4 justify-end">
            <button
              @click="addTask = false"
              class="bg-slate-100 text-slate-500 hover:bg-slate-200 py-1 px-3 rounded-lg flex items-center"
            >
              Cancel
            </button>
            <button class="bg-black text-white py-1 px-3 rounded-lg flex items-center">Save</button>
          </div>
        </div>
        <!-- btn add task -->
        <button
          v-if="!addTask"
          @click="addTask = true"
          class="flex gap-2 text-slate-400 border w-full justify-center py-2 mt-3 rounded-lg bg-slate-100 hover:bg-slate-200"
        >
          <i class="ri-add-circle-fill"></i>Add Task
        </button>
      </div>
    </section>
  </div>
</template>

<style scoped></style>
