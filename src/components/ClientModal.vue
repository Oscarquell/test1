<script setup lang="ts">
import { defineProps, defineEmits, watch, ref } from 'vue'

const props = defineProps<{
  dialog: boolean
  title: string
  confirmText: string
}>()

const emit = defineEmits(['confirm', 'close', 'update:dialog'])

// Создаём локальную копию пропса
const localDialog = ref(props.dialog)

// Следим за внешним пропсом
watch(() => props.dialog, (newVal) => {
  localDialog.value = newVal
})

// Следим за локальной переменной и эмитим изменения наружу
watch(localDialog, (newVal) => {
  emit('update:dialog', newVal)
})

const closeDialog = () => {
  localDialog.value = false
  emit('close')
}

const confirmAction = () => {
  emit('confirm')
  localDialog.value = false
  emit('close')
}
</script>

<template>
  <v-dialog v-model="localDialog" max-width="600px">
    <v-card>
      <v-card-title class="headline">{{ props.title }}</v-card-title>

      <v-card-text>
        <slot></slot>
      </v-card-text>

      <v-card-actions class="d-flex justify-end">
        <v-btn color="grey" variant="text" @click="closeDialog">Отмена</v-btn>
        <v-btn color="primary" @click="confirmAction">{{ props.confirmText }}</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>
