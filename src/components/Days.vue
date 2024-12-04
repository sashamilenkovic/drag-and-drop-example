<script setup lang="ts">
import { ref, computed } from "vue";
import Day from "./Day.vue";
import { dragAndDrop } from "@formkit/drag-and-drop/vue";

const daysList = ref();

interface Day {
  name: string;
  todos: string[];
}

// Here is our "source of truth". If yuou wanted, it might be more elegant to
// use a store to manage this. The implementation details surrounding drag and
// drop would be similar.
const plan = ref<{ days: Day[] }>({
  days: [
    {
      name: "Day 1",
      todos: [],
    },
    {
      name: "Day 2",
      todos: [],
    },
  ],
});

const addDay = () => {
  plan.value.days.push({
    name: `Day ${plan.value.days.length + 1}`,
    todos: [],
  });
};

// Here we are going to create a computed property that simply returns the days
// from our source of truth as well as a setter that will update the source of
// truth.
const days = computed({
  get() {
    return plan.value.days;
  },
  set: (values) => {
    plan.value.days = values;
  },
});

// Instead of using the `useDragAndDrop` hook (which is a composable that
// returns new reactive values), we can use the `dragAndDrop` function directly
// and pass our reactive values (computed property) as the `values` prop.
dragAndDrop({
  parent: daysList,
  values: days,
  accepts: () => {
    return false;
  },
});

function updateDay(day: { name: string; todos: string[] }) {
  days.value = days.value.map((d) => (d.name === day.name ? day : d));
}
</script>

<template>
  <button @click="addDay">Add Day</button>
  <ul
    ref="daysList"
    class="p-8 border-2 border-dashed min-h-[150px] rounded-lg space-y-3"
  >
    <!--Each day component will have its own emitter-->
    <Day
      v-for="day in days"
      :key="day.name"
      :day="day"
      @update:day="updateDay"
    />
  </ul>
</template>
