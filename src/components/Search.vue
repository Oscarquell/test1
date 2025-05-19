<template>
  <v-container>
    <v-row>
      <v-col md="4">
        <v-text-field
          :model-value="modelValue"
          label="Поиск"
          single-line
          hide-details
          :variant="variant"
          :density="density"
          @update:model-value="debounce($event)"
        >
          <template
            v-if="modelValue"
            #append-inner
          >
            <v-btn
              size="x-small"
              icon="clear"
              color="primary"
              @click="clear"
            />
          </template>
          <template
            v-if="showSearchBtn"
            #append
          >
            <v-btn
              size="x-small"
              icon="search"
              color="primary"
              @click="searchItem"
            />
          </template>
        </v-text-field>
      </v-col>
    </v-row>
  </v-container>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue';

const props = defineProps<{
  modelValue?: string;
  showSearchBtn?: boolean;
  debounceTime?: number | string;
  density?: string;
  variant?: string;
}>();

const emit = defineEmits<{
  (e: 'update:model-value', value: string): void;
  (e: 'search', value: string): void;
}>();

const timeoutId = ref<ReturnType<typeof setTimeout> | null>(null);
const searchValue = ref(props.modelValue || '');

const timeout = Number.isNaN(parseInt(props.debounceTime as string)) ? 0 : Number(props.debounceTime);

function clear() {
  emit('update:model-value', '');
  emit('search', '');
  if (timeoutId.value) clearTimeout(timeoutId.value);
}

function searchItem() {
  emit('search', searchValue.value);
}

function handleUpdate(value: string) {
  emit('update:model-value', value);
  searchValue.value = value;
  if (!value && !props.showSearchBtn) {
    searchItem();
  }
}

function debounce(value: string) {
  if (timeoutId.value) {
    clearTimeout(timeoutId.value);
  }
  timeoutId.value = setTimeout(() => handleUpdate(value), timeout);
}
</script>

<style scoped>
.col-md-4 {
  padding: 12px 0;
}
.col-md-2 {
  padding: 12px 0;
}
.mx-2 {
  margin: 0 0 0 8px !important;
}
</style>
