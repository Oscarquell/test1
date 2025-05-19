<script setup lang="ts">
import { defineProps, defineEmits, watch, ref } from 'vue';

const props = defineProps<{
  dialog: boolean;
  title: string;
  confirmText?: string;
}>();

const emit = defineEmits<{
  (e: 'update:dialog', value: boolean): void;
  (e: 'confirm'): void;
  (e: 'close'): void;
}>();

// Локальное состояние диалога
const localDialog = ref(props.dialog);

// Синхронизация внешнего пропса с локальным состоянием
watch(() => props.dialog, (val) => {
  localDialog.value = val;
});

// Эмитим наружу, если локальное состояние изменилось
watch(localDialog, (val) => {
  emit('update:dialog', val);
});

const close = () => {
  localDialog.value = false;
  emit('close');
};

const confirm = () => {
  emit('confirm');
  localDialog.value = false;
  emit('close');
};
</script>

<template>
  <v-dialog v-model="localDialog" max-width="500px" persistent>
    <v-card>
      <v-card-title class="text-h6">
        {{ title }}
      </v-card-title>

      <v-card-text>
        <slot />
      </v-card-text>

      <v-card-actions class="justify-end">
        <v-btn variant="text" @click="close">Отмена</v-btn>
        <v-btn color="primary" @click="confirm">{{ confirmText || 'Сохранить' }}</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>
