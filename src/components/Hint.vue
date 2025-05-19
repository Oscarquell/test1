<template>
  <v-tooltip
    :text="msg"
    :location="left ? 'start' : location"
  >
    <template #activator="{ props }">
      <div v-bind="props">
        <slot />
      </div>
    </template>
  </v-tooltip>
</template>

<script setup lang="ts">
import { defineProps } from 'vue';

defineProps<{
  msg?: string;
  location?: string;
  left?: boolean;
}>();
</script>
