<template>
  <div id="dialog" class="fixed top-0 left-0 z-10 w-full h-full bg-[#00000099]">
    <div class="w-fit bg-[#323940] relative top-[50%] left-[50%] rounded-lg p-4 min-w-[350px] flex flex-col gap-y-6" style="transform: translate(-50%, -50%)">
      <div class="flex justify-between items-center">
        <input
            v-model="title"
            class="bg-[#323940] border-2 border-gray-500 px-2 py-1 rounded focus:border-blue-400 focus:outline-0 text-gray-300"
        >

        <button
            class="text-sm font-medium text-gray-300 flex justify-center items-center rounded hover:bg-gray-600 transition-all w-8 h-8"
            @click="close"
        >
          &#x2715;
        </button>
      </div>

      <div class="flex flex-col gap-y-2 text-sm text-gray-300">
        <span>Description</span>

        <textarea
            v-model="description"
            rows="5" class="rounded border-2 border-gray-500 bg-gray-500 p-1 focus:border-blue-400 focus:outline-0"
        ></textarea>
      </div>

      <div class="flex justify-end">
        <button
            class="bg-blue-400 text-black text-sm rounded px-3 py-2"
            @click="apply"
        >
          Apply changes
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CardDetailDialog',
  data() {
    return {
      title: '',
      description: '',
      cardIndex: null,
      columnIndex: null,
    }
  },
  beforeMount() {
    this.getContentFromLocalStorage()
  },
  mounted() {
    const dialog = document.getElementById('dialog')
    document.addEventListener('click', (event) => {
      if (event.target === dialog)
        this.close()
    })
  },
  methods: {
    getContentFromLocalStorage() {
      this.cardIndex = Number(localStorage.getItem('selectedCardIndex'))
      this.columnIndex = Number(localStorage.getItem('selectedColumnIndex'))
      const column = JSON.parse(localStorage.getItem('columns'))[this.columnIndex]
      this.title = column.cards[this.cardIndex].title
      this.description = column.cards[this.cardIndex].description
    },
    close() {
      this.$emit('close')
    },
    apply() {
      const columns = JSON.parse(localStorage.getItem('columns'))
      columns[this.columnIndex].cards[this.cardIndex].title = this.title
      columns[this.columnIndex].cards[this.cardIndex].description = this.description
      localStorage.setItem('columns', JSON.stringify(columns))
      this.$emit('updateColumns')
      this.close()
    },
  },
}
</script>
