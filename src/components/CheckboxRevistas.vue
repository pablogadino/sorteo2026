<script setup lang="ts">
defineProps<{
  opciones: string[]
  modelValue: string[]
  error?: string
}>()

defineEmits<{
  "update:modelValue": [value: string[]]
}>()

const opcionesRevistas = ["Primaria", "Niño en Obra", "Educación del Pueblo"];
</script>

<template>
  <div class="grupo-campo grupo-checkboxes">
    <label>Revistas que recibe <span class="requerido">*</span></label>
    <div class="checkboxes" :class="{ error }">
      <label v-for="r in opciones" :key="r" class="checkbox-opcion">
        <input
          type="checkbox"
          :value="r"
          :checked="modelValue.includes(r)"
          @change="$emit('update:modelValue', ($event.target as HTMLInputElement).checked ? [...modelValue, r] : modelValue.filter((v: string) => v !== r))"
        />
        <span>{{ r }}</span>
      </label>
    </div>
    <span class="msg-error" v-if="error">{{ error }}</span>
  </div>
</template>

<style scoped>
.grupo-checkboxes {
  grid-column: span 2;
}

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

.checkboxes {
  display: flex;
  flex-wrap: wrap;
  gap: 0.6rem;
  padding: 0.6rem 0.8rem;
  border-radius: 8px;
  border: 1px solid rgba(255, 255, 255, 0.15);
  background: rgba(255, 255, 255, 0.07);
  transition:
    border-color 0.2s,
    box-shadow 0.2s;
}

.checkboxes.error {
  border-color: #e74c3c;
  box-shadow: 0 0 0 3px rgba(231, 76, 60, 0.15);
}

.checkbox-opcion {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  font-size: 0.9rem;
  cursor: pointer;
  color: #ccc;
}

.checkbox-opcion input[type="checkbox"] {
  width: 16px;
  height: 16px;
  accent-color: #f7971e;
  cursor: pointer;
}

.requerido {
  color: #e74c3c;
}

.msg-error {
  font-size: 0.75rem;
  color: #e74c3c;
  margin-top: 0.25rem;
}

@media (max-width: 520px) {
  .grupo-checkboxes {
    grid-column: span 1;
  }
}
</style>
