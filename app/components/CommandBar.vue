<template>
  <div class="fixed inset-0 z-[1000] flex items-center justify-center bg-black/50 backdrop-blur-sm">
    <div
      class="cmdbar-container flex items-center justify-center border border-border shadow-lg rounded-xl bg-bg p2 flex-col gap-2"
    >
      <label for="command-input" class="sr-only">command-input</label>

      <input
        type="text"
        label="Enter command..."
        v-model="inputVal"
        id="command-input"
        class="w-96 h-12 px-4 text-fg outline-none bg-bg-subtle border border-border rounded-md"
        placeholder="Enter command..."
      />

      <div class="w-96 h-48 overflow-auto">
        <div
          v-for="item in filteredCmdList"
          :key="item.id"
          class="flex-col items-center justify-between px-4 py-2 not-first:mt-2 hover:bg-bg-subtle select-none cursor-pointer rounded-md"
          :class="{ 'bg-bg-subtle': item.id === selected }"
        >
          <div class="text-fg">{{ item.name }}</div>
          <div class="text-fg-subtle">{{ item.description }}</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
const cmdList = [
  {
    id: 'npmx',
    name: 'npmx',
    description: 'Run npmx commands',
  },
  {
    id: 'npmx-init',
    name: 'npmx init',
    description: 'Initialize a new npmx project',
  },
  {
    id: 'npmx-install',
    name: 'npmx install',
    description: 'Install npmx dependencies',
  },
  {
    id: 'npmx-run',
    name: 'npmx run',
    description: 'Run npmx scripts',
  },
]

const selected = ref(cmdList[0]?.id || '')
const inputVal = ref('')

const filteredCmdList = computed(() => {
  if (!inputVal.value) {
    return cmdList
  }
  const filter = inputVal.value.trim().toLowerCase()
  return cmdList.filter(
    item =>
      item.name.toLowerCase().includes(filter) ||
      item.description.toLowerCase().includes(filter) ||
      item.id.includes(filter),
  )
})

watch(
  () => filteredCmdList.value,
  newVal => {
    if (newVal.length) {
      selected.value = newVal[0]?.id || ''
    }
  },
)
</script>
