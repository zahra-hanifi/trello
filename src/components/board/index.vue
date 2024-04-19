<template>
  <div class="bg-gray-800 p-4 min-h-[100vh] overflow-x-auto flex gap-x-4">
    <div class="flex gap-x-4">
      <div
          v-for="(column, index) in columns"
          :key="column.id"
          :id="index"
          class="p-4 rounded-2xl bg-gray-950 min-w-64 flex flex-col gap-y-4 h-fit"
          @dragover="allowDrop($event)"
          @drop="dropCard($event)"
      >
        <span class="text-sm text-gray-200 font-medium">{{ column.title }}</span>

        <div v-if="column.cards.length" class="flex flex-col gap-y-2">
          <div
              v-for="(card, cardIndex) in column.cards"
              :key="card.id"
              :id="`${index}-${cardIndex}`"
              draggable="true"
              class="px-2 py-1 rounded-lg bg-gray-800 text-gray-300 border-2 border-gray-800 hover:border-blue-400 cursor-pointer"
              @click="openCardDetailDialog(cardIndex, index)"
              @dragstart="dragCard($event)"
          >
            {{ card.title }}
          </div>
        </div>

        <AddItemSection
            v-if="addCardInputIsVisible && selectedColumnToAddCard.id === column.id"
            button-label="Add card"
            @hide="hideAddCardInput"
            @addItem="addCard"
        />

        <button
            v-else
            class="text-sm text-gray-400 font-medium text-start p-2 rounded-lg hover:bg-gray-800 transition-all"
            @click="showAddCardInput(column)"
        >
          &#43; Add a card
        </button>
      </div>
    </div>

    <AddItemSection
        v-if="addColumnInputIsVisible"
        button-label="Add list"
        class="p-4 rounded-2xl bg-gray-950 min-w-64 flex flex-col gap-y-4 h-fit "
        @hide="toggleAddColumnInput"
        @addItem="addColumn"
    />

    <button
        v-else
        class="px-4 py-3 rounded-2xl bg-gray-700 hover:bg-gray-600 min-w-64 h-fit text-start text-white text-sm font-medium transition-all"
        @click="toggleAddColumnInput"
    >
      &#43; Add another list
    </button>

    <CardDetailDialog
        v-if="isCardDetailDialogVisible"
        @close="closeDialog"
        @updateColumns="setColumns"
    />
  </div>
</template>

<script>
import CardDetailDialog from '@/components/board/CardDetailDialog'
import AddItemSection from '@/components/board/AddItemSection'

export default {
  name: 'BoardComponent',
  components: { CardDetailDialog, AddItemSection },
  data() {
    return {
      columns: [
        {
          id: 1,
          title: 'To Do',
          cards: []
        },
        {
          id: 2,
          title: 'Doing',
          cards: []
        },
        {
          id: 3,
          title: 'Done',
          cards: []
        },
      ],
      addColumnInputIsVisible: false,
      addCardInputIsVisible: false,
      selectedColumnToAddCard: null,
      isCardDetailDialogVisible: false,
    }
  },
  mounted() {
    this.setColumns()
  },
  methods: {
    toggleAddColumnInput() {
      this.addColumnInputIsVisible = !this.addColumnInputIsVisible
    },
    addColumn(title) {
      if (!title) return

      this.columns.push({
        id: this.columns.length + 1,
        title: title,
        cards: []
      })
      this.toggleAddColumnInput()
      this.saveToLocalStorage()
    },
    hideAddCardInput() {
      this.selectedColumnToAddCard = null
      this.addCardInputIsVisible = false
    },
    showAddCardInput(column) {
      this.selectedColumnToAddCard = column
      this.addCardInputIsVisible = true
    },
    addCard(title) {
      if (!title) return

      this.selectedColumnToAddCard.cards.push({
        id: this.selectedColumnToAddCard.cards.length + 1,
        title: title
      })

      this.saveToLocalStorage()
    },
    openCardDetailDialog(cardIndex, columnIndex) {
      localStorage.setItem('selectedCardIndex', cardIndex)
      localStorage.setItem('selectedColumnIndex', columnIndex)
      this.isCardDetailDialogVisible = true
    },
    closeDialog() {
      this.isCardDetailDialogVisible = false
    },
    allowDrop(event) {
      event.preventDefault()
    },
    dragCard(event) {
      event.dataTransfer.setData('card', event.target.id)
    },
    dropCard(event) {
      event.preventDefault()
      let data = event.dataTransfer.getData('card')
      event.target.appendChild(document.getElementById(data))
    },
    setColumns() {
      const columns = JSON.parse(localStorage.getItem('columns'))
      if (columns?.length) {
        this.columns = columns
      } else {
        localStorage.setItem('columns', JSON.stringify(this.columns))
      }
    },
    saveToLocalStorage() {
      localStorage.setItem('columns', JSON.stringify(this.columns))
    },
  },
}
</script>
