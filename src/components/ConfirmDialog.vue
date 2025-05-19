<template>
  <v-dialog
    v-model="localDialog"
    :max-width="maxWidth"
    @update:model-value="handleUpdateDialog"
  >
    <v-card class="pa-3">
      <v-card-title style="text-align: center">
        <v-spacer />
        <!-- eslint-disable-next-line vue/no-v-html -->
        <span v-html="title" />
        <v-spacer />
      </v-card-title>
      <slot />
      <v-card-actions>
        <v-spacer />
        <v-btn
          :loading="loading"
          :disabled="okDisabled"
          class="mr-5"
          color="primary"
          variant="elevated"
          @click="confirm"
        >
          Подтвердить
        </v-btn>
        <v-btn
          :disabled="loading"
          color="error"
          variant="elevated"
          @click="close"
        >
          Отмена
        </v-btn>
        <v-spacer />
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  name: 'ConfirmDialog',
  props: {
    modelValue: {
      type: [Boolean, Function],
      default: false,
    },
    title: {
      type: String,
      default: 'Вы уверены ?',
    },
    maxWidth: {
      type: String,
      default: '300px',
    },
    okDisabled: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      loading: false,
      localDialog: !!this.modelValue,
    };
  },
  watch: {
    modelValue(val) {
      this.localDialog = !!val;
    },
    localDialog(val) {
      this.$emit('update:modelValue', val);
    },
  },
  methods: {
    close() {
      this.localDialog = false;
      this.loading = false;
      this.$emit('handleCancel');
    },
    handleUpdateDialog(val) {
      if (!val) {
        this.$emit('handleCancel');
      }
    },
    async confirm() {
      this.loading = true;
      if (typeof this.modelValue === 'function') {
        this.modelValue().finally(this.close);
      } else {
        await this.$emit('handleOk', () => {
          this.loading = false;
          this.localDialog = false;
        });
      }
    },
  },
};
</script>

<style scoped>
</style>
