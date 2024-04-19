<template>
  <div class="flex flex-col gap-y-3">
    <input
        v-model="value"
        placeholder="Enter a title for this card..."
        autofocus
        class="bg-gray-700 px-2 py-1 rounded text-white focus:outline-0 border-2 border-gray-700 focus:border-blue-400 text-sm"
    />

    <div class="flex items-center gap-x-2">
      <button
          class="bg-blue-400 text-black text-sm rounded px-3 py-2"
          @click="addItem"
      >
        {{ buttonLabel }}
      </button>

      <button
          class="text-sm font-medium text-gray-200 w-8 h-8 flex justify-center items-center rounded hover:bg-gray-800 transition-all"
          @click="hide"
      >
        &#x2715;
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AddItemSection',
  props: {
    buttonLabel: { type: String, default: 'Add' }
  },
  data() {
    return {
      value: '',
    }
  },
  methods: {
    hide() {
      this.$emit('hide')
    },
    addItem() {
      this.$emit('addItem', this.value)
      this.value = ''
    }
  },
}
</script>