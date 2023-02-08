<script setup lang="ts">
  import { computed } from "vue"
  import { parseISO, lightFormat } from "date-fns"

  const props = defineProps<{
    title: string
    deadline: string
    status: string
  }>()

  const formattedDeadline = computed(() => lightFormat(parseISO(props.deadline), 'yyyy-MM-dd'))
</script>

<template>
  <a href="#" class="block max-w-sm p-6 bg-white border border-gray-200 rounded-lg shadow hover:bg-gray-100 dark:bg-gray-800 dark:border-gray-700 dark:hover:bg-gray-700">
    <h5 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white line-clamp-3">{{ title }}</h5>
    <div class="grid grid-flow-col justify-between">
      <p class="font-normal text-gray-700 dark:text-gray-400">Due to {{ formattedDeadline }}</p>

      <p class="font-normal text-gray-700 dark:text-gray-400">Status: {{ status }}</p>
    </div>
  </a>
</template>