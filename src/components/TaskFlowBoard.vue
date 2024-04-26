<script setup lang="ts">
import type { Column } from '@/types'
import { nanoid } from 'nanoid'
import { ref } from 'vue'
import TaskFlowTask from './TaskFlowTask.vue'
import draggable from 'vuedraggable'
import DragHandle from './DragHandle.vue'

const columns = ref<Column[]>([
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
</script>

<template>
  <!-- can't add the handle option because the DnD stop working -->
  <div>
    <draggable
      v-model="columns"
      group="columns"
      item-key="id"
      :animation="150"
      class="flex gap-4 overflow-x-auto items-start"
    >
      <template #item="{ element: column }: { element: Column }">
        <div class="column bg-gray-200 p-5 rounded min-w-[250px]">
          <header class="font-bold mb-4">
            <DragHandle />
            {{ column.title }}
          </header>
          <TaskFlowTask v-for="task in column.tasks" :key="task.id" :task="task" />
          <footer>
            <button class="text-gray-500">+ Ajouter une tache</button>
          </footer>
        </div>
      </template>
    </draggable>
  </div>
</template>
