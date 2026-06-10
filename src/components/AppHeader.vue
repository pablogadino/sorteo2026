<script setup lang="ts">
</script>

<template>
  <div>
    <div class="alerta-prueba">AULA - REVISTA DE LA EDUCACIÓN DEL PUEBLO</div>
    <header class="heroe">
      <div class="emoji">🎁</div>
      <h1>Sorteo Primaria-Niño en Obra 2026</h1>
      <p class="exclusividad">
        Sorteo exclusivo para personas con suscripción vigente de las revistas de Aula.
      </p>
      <p class="disclaimer">
        El sistema no tomará en cuenta formularios llenados por personas sin suscripción vigente.
      </p>
    </header>
  </div>
</template>

<style scoped>
.alerta-prueba {
  text-align: center;
  background: #ffc107;
  color: #1a1a2e;
  font-weight: 800;
  font-size: 0.8rem;
  letter-spacing: 0.15em;
  padding: 0.4rem;
  border-radius: 6px;
  margin-bottom: 1.2rem;
}

.heroe {
  text-align: center;
  padding: 2rem 0 1.5rem;
}

.emoji {
  font-size: 3.5rem;
  margin-bottom: 0.5rem;
}

h1 {
  font-size: 2.2rem;
  font-weight: 800;
  background: linear-gradient(90deg, #f7971e, #ffd200);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.exclusividad {
  margin-top: 0.8rem;
  font-size: 0.9rem;
  color: #f7971e;
  font-weight: 600;
}

.disclaimer {
  margin-top: 0.5rem;
  font-size: 0.78rem;
  color: #999;
  font-style: italic;
  max-width: 520px;
  margin-left: auto;
  margin-right: auto;
}

@media (max-width: 520px) {
  h1 {
    font-size: 1.7rem;
  }
}
</style>
