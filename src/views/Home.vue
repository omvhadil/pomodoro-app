<!-- eslint-disable vue/multi-word-component-names -->
<script setup>
import { ref, watch, reactive, watchEffect } from "vue";
import { useDark, useToggle } from "@vueuse/core";

const isDark = useDark();
const toggleDark = useToggle(isDark);

const db = reactive({
  data: JSON.parse(localStorage.getItem("tasks") || "[]"),
  task: "",
  mode: "add",
  selected: null,
});
const addTask = ref(false);
const fitur = ref("working");
const workMinute = ref(25);
const workSecond = ref(0);
const shortMinute = ref(5);
const shortSecond = ref(0);
const longMinute = ref(15);
const longSecond = ref(0);
const button = ref(false);
const toggleTask = ref(false);
let timeout;

const createTasks = () => {
  if (db.mode === "add") {
    db.data.push({
      task: db.task,
    }),
      (db.task = ""),
      (addTask.value = false);
  } else {
    // mode edit
    db.data[db.selected].task = db.task;
    db.task = "";
    addTask.value = false;
    db.mode = "add";
  }
};

const delTasks = (index) => {
  db.data.splice(index, 1);
};

const delAllTasks = () => {
  db.data = [];
};

watchEffect(() => {
  localStorage.setItem("tasks", JSON.stringify(db.data));
});

const shound = new Audio(
  "https://assets.mixkit.co/sfx/preview/mixkit-clock-tick-tock-fast-916.mp3"
);

const timeCount = () => {
  if (fitur.value === "working") {
    workSecond.value--;
  } else if (fitur.value === "shortB") {
    shortSecond.value--;
  } else if (fitur.value === "longB") {
    longSecond.value--;
  }
  timeout = setTimeout(timeCount, 1000);
};

function startCount() {
  if (!button.value) {
    button.value = true;
    timeCount();
  }
}

function stopCount() {
  clearTimeout(timeout);
  button.value = false;
}

const resetWork = () => {
  stopCount();
  workMinute.value = 25;
  workSecond.value = 0;
  shortMinute.value = 5;
  shortSecond.value = 0;
  longMinute.value = 15;
  longSecond.value = 0;
};

watch(workSecond, () => {
  if (workSecond.value === -1) {
    workSecond.value = 59;
    workMinute.value--;
  }
});
watch(shortSecond, () => {
  if (shortSecond.value === -1) {
    shortSecond.value = 59;
    shortMinute.value--;
  }
});
watch(longSecond, () => {
  if (longSecond.value === -1) {
    longSecond.value = 59;
    longMinute.value--;
  }
});

watch(workMinute, () => {
  if (workMinute.value === -1) {
    workMinute.value = 0;
    workSecond.value = 0;
    shound.play();
    resetWork();
  }
});
watch(shortMinute, () => {
  if (shortMinute.value === -1) {
    shortMinute.value = 0;
    shortSecond.value = 0;
    shound.play();
    resetWork();
  }
});
watch(longMinute, () => {
  if (longMinute.value === -1) {
    longMinute.value = 0;
    longSecond.value = 0;
    shound.play();
    resetWork();
  }
});

const formatTime = (time) => {
  if (time < 10) {
    return `0${time}`;
  }
  return time;
};

const formatTheme = (theme) => {
  switch (theme) {
    case "working":
      return "bg-gradient-to-t from-emerald-600 to-emerald-400";
    case "shortB":
      return "bg-gradient-to-t from-amber-600 to-amber-400";
    case "longB":
      return "bg-gradient-to-t from-sky-600 to-sky-400";
    default:
      return "bg-gradient-to-t from-emerald-600 to-emerald-400";
  }
};

const formatTheme2 = (theme) => {
  switch (theme) {
    case "working":
      return "border-emerald-500";
    case "shortB":
      return "border-amber-500";
    case "longB":
      return "border-sky-500";
    default:
      return "border-emerald-500";
  }
};

const formatTitle = (title) => {
  switch (title) {
    case "working":
      return "Work Session";
    case "shortB":
      return "Short Break";
    case "longB":
      return "Long Break";
    default:
      return "Work Session";
  }
};
</script>

<template>
  <div class="w-full h-[40%] absolute" :class="formatTheme(fitur)"></div>
  <div class="w-full absolute top-[30%] md:top-[24%] lg:top-[17%]">
    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1440 320" class="dark:hidden">
      <path
        fill="#ffffff"
        fill-opacity="1"
        d="M0,128L30,138.7C60,149,120,171,180,186.7C240,203,300,213,360,186.7C420,160,480,96,540,96C600,96,660,160,720,170.7C780,181,840,139,900,122.7C960,107,1020,117,1080,133.3C1140,149,1200,171,1260,160C1320,149,1380,107,1410,85.3L1440,64L1440,320L1410,320C1380,320,1320,320,1260,320C1200,320,1140,320,1080,320C1020,320,960,320,900,320C840,320,780,320,720,320C660,320,600,320,540,320C480,320,420,320,360,320C300,320,240,320,180,320C120,320,60,320,30,320L0,320Z"
      ></path>
    </svg>
    <svg
      xmlns="http://www.w3.org/2000/svg"
      viewBox="0 0 1440 320"
      class="hidden dark:block"
    >
      <path
        fill="#111827"
        fill-opacity="1"
        d="M0,128L30,138.7C60,149,120,171,180,186.7C240,203,300,213,360,186.7C420,160,480,96,540,96C600,96,660,160,720,170.7C780,181,840,139,900,122.7C960,107,1020,117,1080,133.3C1140,149,1200,171,1260,160C1320,149,1380,107,1410,85.3L1440,64L1440,320L1410,320C1380,320,1320,320,1260,320C1200,320,1140,320,1080,320C1020,320,960,320,900,320C840,320,780,320,720,320C660,320,600,320,540,320C480,320,420,320,360,320C300,320,240,320,180,320C120,320,60,320,30,320L0,320Z"
      ></path>
    </svg>
  </div>
  <main class="main max-w-[550px] h-[100vh] mx-auto relative">
    <!-- header -->
    <header class="header w-full">
      <div class="container h-14 flex items-center justify-between">
        <a href="#" class="font-bold text-white">Pomodoro</a>
        <div class="flex gap-5">
          <div @click="toggleDark()">
            <button v-if="isDark" class="text-white text-lg">
              <i class="ri-sun-line"></i>
            </button>
            <button v-else class="text-white text-lg">
              <i class="ri-moon-line"></i>
            </button>
          </div>
          <button class="text-white text-lg">
            <i class="ri-settings-3-line"></i>
          </button>
        </div>
      </div>
    </header>
    <!-- fitur -->
    <section class="fitur">
      <div class="flex justify-center gap-4 mt-8 text-white">
        <button
          @click="(fitur = 'working'), resetWork()"
          :class="fitur === 'working' ? 'font-bold' : ''"
        >
          Working
        </button>
        <button
          @click="(fitur = 'shortB'), resetWork()"
          :class="fitur === 'shortB' ? 'font-bold' : ''"
        >
          Short Break
        </button>
        <button
          @click="(fitur = 'longB'), resetWork()"
          :class="fitur === 'longB' ? 'font-bold' : ''"
        >
          Long Break
        </button>
      </div>
    </section>
    <!-- clock -->
    <section class="clock">
      <div
        class="w-[250px] h-[250px] bg-white dark:bg-gray-900 rounded-full mx-auto shadow-md mt-8 flex items-center justify-center"
      >
        <div
          class="w-[220px] h-[220px] bg-white dark:bg-gray-900 rounded-full border-8 flex items-center justify-center"
          :class="formatTheme2(fitur)"
        >
          <section class="work-session" v-if="fitur === 'working'">
            <div class="text-center leading-10">
              <p class="text-slate-400">Work Session</p>
              <h1 class="text-5xl font-semibold dark:text-white">
                {{ formatTime(workMinute) }} : {{ formatTime(workSecond) }}
              </h1>
              <p class="text-slate-400">Minute - Second</p>
            </div>
          </section>
          <section class="short-break" v-if="fitur === 'shortB'">
            <div class="text-center leading-10">
              <p class="text-slate-400">Short Break</p>
              <h1 class="text-5xl font-semibold dark:text-white">
                {{ formatTime(shortMinute) }} : {{ formatTime(shortSecond) }}
              </h1>
              <p class="text-slate-400">Minute - Second</p>
            </div>
          </section>
          <section class="long-break" v-if="fitur === 'longB'">
            <div class="text-center leading-10">
              <p class="text-slate-400">Long Break</p>
              <h1 class="text-5xl font-semibold dark:text-white">
                {{ formatTime(longMinute) }} : {{ formatTime(longSecond) }}
              </h1>
              <p class="text-slate-400">Minute - Second</p>
            </div>
          </section>
        </div>
      </div>
    </section>
    <!-- button start -->
    <section class="button-start">
      <div class="container mt-4">
        <div class="text-center">
          <h6 class="dark:text-white">Time {{ formatTitle(fitur) }}</h6>
        </div>
        <div class="mt-6 flex justify-center">
          <section class="work-session" v-if="fitur === 'working'">
            <button
              v-if="!button"
              class="text-white py-3 px-16 rounded-lg"
              :class="formatTheme(fitur)"
              @click="startCount()"
            >
              START
            </button>
            <button
              v-if="button"
              class="text-white py-3 px-16 rounded-lg bg-gradient-to-t from-red-600 to-red-400"
              :class="formatTheme(fitur)"
              @click="stopCount()"
            >
              STOP
            </button>
          </section>
          <section class="short-break" v-if="fitur === 'shortB'">
            <button
              v-if="!button"
              class="text-white py-3 px-16 rounded-lg"
              :class="formatTheme(fitur)"
              @click="startCount()"
            >
              START
            </button>
            <button
              v-if="button"
              class="text-white py-3 px-16 rounded-lg bg-gradient-to-t from-red-600 to-red-400"
              :class="formatTheme(fitur)"
              @click="stopCount()"
            >
              STOP
            </button>
          </section>
          <section class="long-break" v-if="fitur === 'longB'">
            <button
              v-if="!button"
              class="text-white py-3 px-16 rounded-lg"
              :class="formatTheme(fitur)"
              @click="startCount()"
            >
              START
            </button>
            <button
              v-if="button"
              class="text-white py-3 px-16 rounded-lg bg-gradient-to-t from-red-600 to-red-400"
              :class="formatTheme(fitur)"
              @click="stopCount()"
            >
              STOP
            </button>
          </section>
        </div>
      </div>
    </section>
    <!-- tasks -->
    <section class="tasks">
      <div class="container mt-6">
        <div class="shadow-lg rounded-lg p-4">
          <div class="flex justify-between pb-2">
            <span class="dark:text-white">Tasks</span>
            <div class="relative inline-block text-left">
              <div>
                <button @click="toggleTask = !toggleTask" class="dark:text-white">
                  <i class="ri-more-2-fill"></i>
                </button>
              </div>
              <div
                v-if="toggleTask"
                class="absolute right-0 z-10 mt-2 w-56 origin-top-right rounded-md bg-white shadow-lg ring-1 ring-black ring-opacity-5 focus:outline-none"
                role="menu"
                aria-orientation="vertical"
                aria-labelledby="menu-button"
                tabindex="-1"
              >
                <div class="py-1" role="none">
                  <div
                    class="text-gray-700 flex justify-between gap-2 px-4 py-2 text-sm"
                    role="menuitem"
                    tabindex="-1"
                    id="menu-item-0"
                  >
                    <span>Save as template </span
                    ><i class="ri-git-repository-private-fill"></i>
                  </div>
                  <div
                    class="text-gray-700 flex gap-2 justify-between px-4 py-2 text-sm"
                    role="menuitem"
                    tabindex="-1"
                    id="menu-item-1"
                  >
                    <span>Add from template </span
                    ><i class="ri-git-repository-private-fill"></i>
                  </div>
                  <div
                    class="text-gray-700 flex gap-2 justify-between px-4 py-2 text-sm"
                    role="menuitem"
                    tabindex="-1"
                    id="menu-item-2"
                  >
                    <span>Import from Todo Tasks </span
                    ><i class="ri-git-repository-private-fill"></i>
                  </div>
                  <div
                    class="text-gray-700 flex gap-2 justify-between px-4 py-2 text-sm"
                    role="menuitem"
                    tabindex="-1"
                    id="menu-item-2"
                    @click="delAllTasks()"
                  >
                    <span>Clear all tasks </span>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <hr />
          <div class="mt-2 grid grid-cols-1 gap-2">
            <!-- list task -->
            <div
              v-for="(item, index) in db.data"
              :key="index"
              class="flex justify-between bg-slate-200 py-3 px-2 rounded-md dark:bg-slate-700"
            >
              <span class="font-semibold text-slate-600 dark:text-slate-100">{{
                item.task
              }}</span>
              <div class="flex text-slate-500">
                <button
                  @click="
                    (db.mode = 'edit'),
                      (db.selected = index),
                      (addTask = true),
                      (db.task = item.task)
                  "
                  class="px-2"
                  title="Edit"
                >
                  <i class="ri-edit-2-line"></i>
                </button>
                <button @click="delTasks(index)" class="px-2" title="Hapus">
                  <i class="ri-delete-bin-6-line"></i>
                </button>
              </div>
            </div>
          </div>
          <!-- card add task -->
          <div
            v-if="addTask"
            class="border w-full mt-2 rounded-lg p-3 dark:border-slate-500 dark:bg-slate-800"
          >
            <form @submit.prevent="createTasks()">
              <input
                type="text"
                placeholder="What are you working on?"
                class="focus:outline-none w-full dark:bg-slate-800 dark:text-white"
                v-model="db.task"
              />
              <div class="flex gap-2 mt-4 justify-end">
                <button
                  type="button"
                  @click="addTask = false"
                  class="bg-slate-100 text-slate-500 dark:bg-slate-400 dark:text-slate-100 hover:bg-slate-200 py-1 px-3 rounded-lg flex items-center"
                >
                  Cancel
                </button>
                <button
                  type="submit"
                  class="bg-black text-white py-1 px-3 rounded-lg flex items-center"
                >
                  Save
                </button>
              </div>
            </form>
          </div>
          <!-- btn add task -->
          <button
            v-if="!addTask"
            @click="addTask = true"
            class="flex gap-2 text-slate-400 dark:bg-slate-800 dark:text-slate-500 border dark:border-slate-400 w-full justify-center py-2 mt-3 rounded-lg bg-slate-100 hover:bg-slate-200"
          >
            <i class="ri-add-circle-fill"></i>Add Task
          </button>
        </div>
      </div>
    </section>
  </main>
</template>

<style scoped></style>
