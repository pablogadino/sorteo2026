<script setup lang="ts">
defineProps<{
  id: string
  label: string
  type?: string
  inputmode?: 'text' | 'search' | 'email' | 'tel' | 'url' | 'none' | 'numeric' | 'decimal'
  maxlength?: string
  error?: string
  modelValue: string
}>()

defineEmits<{
  "update:modelValue": [value: string]
}>()
</script>

<template>
  <div class="grupo-campo">
    <label :for="id">{{ label }} <span class="requerido">*</span></label>
    <input
      :id="id"
      :type="type ?? 'text'"
      :inputmode="inputmode"
      :maxlength="maxlength"
      :value="modelValue"
      :class="{ error }"
      @input="$emit('update:modelValue', ($event.target as HTMLInputElement).value)"
    />
    <span class="msg-error" v-if="error">{{ error }}</span>
  </div>
</template>

<style scoped>
.grupo-campo {
  display: flex;
  flex-direction: column;
}

label {
  font-size: 0.85rem;
  font-weight: 600;
  margin-bottom: 0.3rem;
  color: #aaa;
}

input {
  padding: 0.65rem 0.8rem;
  border-radius: 8px;
  border: 1px solid rgba(255, 255, 255, 0.15);
  background: rgba(255, 255, 255, 0.07);
  color: #e0e0e0;
  font-size: 0.95rem;
  font-family: inherit;
  transition:
    border-color 0.2s,
    box-shadow 0.2s;
  outline: none;
}

input:focus {
  border-color: #f7971e;
  box-shadow: 0 0 0 3px rgba(247, 151, 30, 0.2);
}

input.error {
  border-color: #e74c3c;
  box-shadow: 0 0 0 3px rgba(231, 76, 60, 0.15);
}

.requerido {
  color: #e74c3c;
}

.msg-error {
  font-size: 0.75rem;
  color: #e74c3c;
  margin-top: 0.25rem;
}
</style>
