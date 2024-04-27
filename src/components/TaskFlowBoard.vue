<script setup lang="ts">
import type { Column, Task } from '@/types'
import { nanoid } from 'nanoid'
import { nextTick, ref, watch } from 'vue'
import TaskFlowTask from './TaskFlowTask.vue'
import draggable from 'vuedraggable'
import DragHandle from './DragHandle.vue'
import { useKeyModifier, useLocalStorage } from '@vueuse/core'
import NewTask from './NewTask.vue'

const columns = useLocalStorage<Column[]>('taskflowBoard', [
  {
    id: nanoid(),
    title: 'Backlog',
    tasks: [
      {
        id: nanoid(),
        title: 'Intégration de téléchargement et de relecture',
        createdAt: new Date()
      },
      {
        id: nanoid(),
        title: 'Revoir la modale Informations complémentaires',
        createdAt: new Date()
      }
    ]
  },
  {
    id: nanoid(),
    title: 'Erroné',
    tasks: [
      {
        id: nanoid(),
        title: "Devis - Choisir l'ordre des prestations",
        createdAt: new Date()
      },
      {
        id: nanoid(),
        title: 'Connectivité labos',
        createdAt: new Date()
      }
    ]
  },
  {
    id: nanoid(),
    title: 'Relecture',
    tasks: [
      {
        id: nanoid(),
        title: "Bug à l'initialisation de la facture",
        createdAt: new Date()
      },
      {
        id: nanoid(),
        title: 'Révision sur factures',
        createdAt: new Date()
      },
      {
        id: nanoid(),
        title: 'Faire remonter les infos règlements',
        createdAt: new Date()
      }
    ]
  },
  {
    id: nanoid(),
    title: 'A faire',
    tasks: []
  },
  {
    id: nanoid(),
    title: 'En cours',
    tasks: []
  },
  {
    id: nanoid(),
    title: 'A tester DEV',
    tasks: []
  },
  {
    id: nanoid(),
    title: 'A tester PO',
    tasks: []
  }
])

const alt = useKeyModifier('Alt')
function createColumn() {
  const column: Column = {
    id: nanoid(),
    title: '',
    tasks: []
  }

  columns.value.push(column)
  nextTick(() => {
    ;(document.querySelector('.column:last-of-type .title-input') as HTMLInputElement).focus()
  })
}

watch(
  columns,
  () => {
    // ajax request
  },
  {
    deep: true
  }
)
</script>

<template>
  <!-- can't add the handle option because the DnD stop working -->
  <div class="flex items-start overflow-x-auto gap-4">
    <draggable
      v-model="columns"
      group="columns"
      item-key="id"
      :animation="150"
      class="flex gap-4 items-start"
    >
      <template #item="{ element: column }: { element: Column }">
        <div class="column bg-gray-200 p-5 rounded min-w-[250px]">
          <header class="font-bold mb-4">
            <DragHandle />
            <input
              class="title-input bg-transparent focus:bg-white rounded px-1 w-4/5"
              @keyup.enter="($event.target as HTMLInputElement).blur()"
              @keydown.backspace="
                column.title === '' ? (columns = columns.filter((c) => c.id !== column.id)) : null
              "
              type="text"
              v-model="column.title"
            />
          </header>
          <draggable
            v-model="column.tasks"
            :animate="150"
            item-key="id"
            :group="{ name: 'tasks', pull: alt ? 'clone' : true }"
          >
            <template #item="{ element: task }: { element: Task }">
              <div>
                <TaskFlowTask
                  :task="task"
                  @delete="column.tasks = column.tasks.filter((t) => t.id !== $event)"
                />
              </div>
            </template>
          </draggable>
          <footer>
            <NewTask @add="column.tasks.push($event)" />
          </footer>
        </div>
      </template>
    </draggable>
    <button @click="createColumn" class="bg-gray-200 whitespace-nowrap p-2 rounded opacity-50">
      + Ajouter une colonne
    </button>
  </div>
</template>
