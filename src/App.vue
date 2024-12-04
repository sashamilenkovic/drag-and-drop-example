<script setup lang="ts">
import Days from "./components/Days.vue";
import { useDragAndDrop } from "@formkit/drag-and-drop/vue";
import { onMounted } from "vue";

const todoItems = ["Make Test", "Make Controllers", "Deploy Project"];

onMounted(async () => {
  todoItems;
});

const [todoList, todos] = useDragAndDrop(todoItems, {
  group: "todoList",
  sortable: false,
  draggingClass: "bg-blue-200 text-white",
});
</script>

<template>
  <div class="py-10 px-20">
    <div class="grid gap-10 grid-cols-2">
      <div class="max-h-[550px] overflow-y-auto bg-white rounded-xl p-10">
        <ul ref="todoList" class="my-10">
          <li
            v-for="todo in todos"
            :key="todo"
            class="p-4 border bg-gray-200 border-gray-300 rounded cursor-grab mb-2 last:mb-0"
          >
            {{ todo }}
          </li>
        </ul>
      </div>
      <Days />
    </div>
  </div>
</template>
