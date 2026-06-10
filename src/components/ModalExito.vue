<script setup lang="ts">
defineProps<{
  nombre: string;
  apellido: string;
}>();

defineEmits<{
  cerrar: [];
}>();
</script>

<template>
  <div class="modal-fondo" @click.self="$emit('cerrar')">
    <div class="modal">
      <div class="icono-proceso">⏳</div>
      <h2>Inscripción en proceso</h2>
      <p>
        <strong>{{ nombre }} {{ apellido }}</strong
        >, los datos se enviaron para evaluar.
      </p>
      <p class="modal-texto">
        El sistema corroborará que el formulario corresponde a personas con
        suscripción vigente a las revistas de Aula. Si no tienes suscripción
        vigente, en los próximos días te enviará la notificación de que no es
        válido al correo. El sorteo es el día 31 de julio entre los formularios
        recibidos que corresponden a suscripciones vigentes. Gracias por
        participar.
      </p>
      <button class="btn-cerrar" @click="$emit('cerrar')">Cerrar</button>
    </div>
  </div>
</template>

<style scoped>
.modal-fondo {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.7);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 1.5rem;
}

.modal {
  text-align: center;
  background: #1a1a2e;
  border: 1px solid rgba(255, 255, 255, 0.15);
  border-radius: 16px;
  padding: 2.5rem 2rem;
  max-width: 520px;
  width: 100%;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
}

.modal h2 {
  font-size: 1.4rem;
  margin-bottom: 0.8rem;
  color: #f7971e;
}

.modal p {
  font-size: 0.95rem;
  color: #ccc;
  line-height: 1.5;
}

.modal-texto {
  margin-top: 0.8rem;
}

.icono-proceso {
  font-size: 3rem;
  margin-bottom: 0.5rem;
}

.btn-cerrar {
  margin-top: 1.5rem;
  padding: 0.65rem 2rem;
  font-size: 0.9rem;
  font-weight: 600;
  font-family: inherit;
  color: #1a1a2e;
  background: linear-gradient(90deg, #f7971e, #ffd200);
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition:
    transform 0.15s,
    box-shadow 0.15s;
}

.btn-cerrar:hover {
  transform: translateY(-1px);
  box-shadow: 0 6px 20px rgba(247, 151, 30, 0.35);
}
</style>
