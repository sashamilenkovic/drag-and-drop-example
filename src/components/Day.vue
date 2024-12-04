<script setup lang="ts">
import Todo from "./Todo.vue";
import { computed, ref } from "vue";
import { dragAndDrop } from "@formkit/drag-and-drop/vue";

const props = defineProps<{
  day: {
    name: string;
    todos: string[];
  };
}>();

const todoList = ref();

const emit = defineEmits(["update:day"]);

// Here we are going to create a computed property that simply returns the todos
// from our source of truth as well as a setter that will update the source of
// truth. Because we are updating the setter in Days.vue, which in turn will
// update our "source of truth", we can count on our computed getter to always
// return the latest todos.
const todos = computed({
  get: () => props.day.todos,
  set: (value) => {
    emit("update:day", {
      ...props.day,
      todos: value,
    });
  },
});

dragAndDrop({
  parent: todoList,
  values: todos,
  group: "todoList",
});
</script>

<template>
  <li class="flex flex-col p-4 bg-gray-200 border border-gray-300 rounded">
    <span> {{ day.name }}</span>
    <h1 class="font-bold">TODO'S</h1>
    <ul
      ref="todoList"
      class="p-8 border-2 border-dashed min-h-[150px] rounded-lg space-y-3"
    >
      <Todo
        v-for="dayTodo in day.todos"
        :key="dayTodo"
        :todo="dayTodo"
        class="p-4 bg-gray-200 border border-gray-300 rounded"
      />
    </ul>
  </li>
</template>
