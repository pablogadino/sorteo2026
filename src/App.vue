<script setup lang="ts">
import { ref, reactive } from 'vue'

const campos = reactive({
  nombre: '',
  apellido: '',
  celular: '',
  email: '',
  cedula: '',
  revistas: [] as string[],
})

const errores = reactive<Record<string, string>>({})
const enviado = ref(false)
const enviando = ref(false)

const opcionesRevistas = [
  'Primaria',
  'Niño en Obra',
  'Educación del Pueblo',
]

const errorServidor = ref('')

const validar = (): boolean => {
  Object.keys(errores).forEach((k) => delete errores[k])
  errorServidor.value = ''

  if (!campos.nombre.trim()) errores.nombre = 'Ingrese su nombre'
  if (!campos.apellido.trim()) errores.apellido = 'Ingrese su apellido'
  if (!campos.celular.trim()) errores.celular = 'Ingrese su celular'
  else if (!/^\d{7,15}$/.test(campos.celular.replace(/\D/g, '')))
    errores.celular = 'Ingrese un número válido'
  if (!campos.email.trim()) errores.email = 'Ingrese su email'
  else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(campos.email))
    errores.email = 'Ingrese un email válido'
  if (!campos.cedula.trim()) errores.cedula = 'Ingrese su cédula'
  else if (!/^\d{8}$/.test(campos.cedula.replace(/\D/g, '')))
    errores.cedula = 'La cédula debe tener 8 dígitos'
  if (campos.revistas.length === 0) errores.revistas = 'Seleccione al menos una revista'

  return Object.keys(errores).length === 0
}

const enviar = async () => {
  if (!validar()) return
  enviando.value = true

  try {
    const response = await fetch('https://aula.com.uy/larete/api/sorteo/2026', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        nombre: campos.nombre.trim(),
        apellido: campos.apellido.trim(),
        celular: campos.celular.trim(),
        email: campos.email.trim(),
        cedula: campos.cedula.trim(),
        revistas: campos.revistas,
      }),
    })

    if (!response.ok) {
      const data = await response.json()
      if (data.errors) {
        for (const [campo, mensajes] of Object.entries(data.errors)) {
          errores[campo] = Array.isArray(mensajes) ? (mensajes as string[])[0] : String(mensajes)
        }
      }
      errorServidor.value = data.message || 'Error al enviar el formulario.'
      return
    }

    enviado.value = true
  } catch (err) {
    if (err instanceof TypeError && err.message === 'Failed to fetch') {
      console.log('Error de red: no se pudo alcanzar el servidor (posible CORS, DNS, o servidor caído).', err)
    } else if (err instanceof Error) {
      console.log('Error inesperado al enviar formulario:', err.message, err)
    } else {
      console.log('Error desconocido al enviar formulario:', err)
    }
    errorServidor.value = 'Error de conexión. Verifique su internet e intente nuevamente.'
  } finally {
    enviando.value = false
  }
}
</script>

<template>
  <div class="contenedor">
    <div class="alerta-prueba">SITIO DE PRUEBAS — NO OFICIAL</div>

    <header class="heroe">
      <div class="emoji">🎁</div>
      <h1>Sorteo Primaria-Niño en Obra 2026</h1>
      <p class="exclusividad">Sorteo exclusivo para personas con suscripción vigente de las revistas de Aula.</p>
      <p class="disclaimer">El sistema no tomará en cuenta formularios llenados por personas sin suscripción vigente (por seguridad de la base de datos no te informa en el momento si no tienes suscripciones).</p>
    </header>

    <main class="formulario-contenedor" v-if="!enviado">
      <h2>Formulario de Participación</h2>

      <div class="grid-campos">
        <div class="grupo-campo">
          <label for="nombre">Nombre <span class="requerido">*</span></label>
          <input
            id="nombre"
            v-model="campos.nombre"
            type="text"
            :class="{ error: errores.nombre }"
          />
          <span class="msg-error" v-if="errores.nombre">{{ errores.nombre }}</span>
        </div>

        <div class="grupo-campo">
          <label for="apellido">Apellido <span class="requerido">*</span></label>
          <input
            id="apellido"
            v-model="campos.apellido"
            type="text"
            :class="{ error: errores.apellido }"
          />
          <span class="msg-error" v-if="errores.apellido">{{ errores.apellido }}</span>
        </div>

        <div class="grupo-campo">
          <label for="celular">Celular <span class="requerido">*</span></label>
          <input
            id="celular"
            v-model="campos.celular"
            type="tel"
            :class="{ error: errores.celular }"
          />
          <span class="msg-error" v-if="errores.celular">{{ errores.celular }}</span>
        </div>

        <div class="grupo-campo">
          <label for="email">Email <span class="requerido">*</span></label>
          <input
            id="email"
            v-model="campos.email"
            type="email"
            :class="{ error: errores.email }"
          />
          <span class="msg-error" v-if="errores.email">{{ errores.email }}</span>
        </div>

        <div class="grupo-campo">
          <label for="cedula">Cédula <span class="requerido">*</span></label>
          <input
            id="cedula"
            v-model="campos.cedula"
            type="text"
            inputmode="numeric"
            maxlength="8"
            :class="{ error: errores.cedula }"
          />
          <span class="msg-error" v-if="errores.cedula">{{ errores.cedula }}</span>
        </div>

        <div class="grupo-campo grupo-checkboxes">
          <label>Revistas que recibe <span class="requerido">*</span></label>
          <div class="checkboxes" :class="{ error: errores.revistas }">
            <label v-for="r in opcionesRevistas" :key="r" class="checkbox-opcion">
              <input type="checkbox" :value="r" v-model="campos.revistas" />
              <span>{{ r }}</span>
            </label>
          </div>
          <span class="msg-error" v-if="errores.revistas">{{ errores.revistas }}</span>
        </div>
      </div>

      <div v-if="errorServidor" class="error-servidor">{{ errorServidor }}</div>

      <button class="btn-enviar" :disabled="enviando" @click="enviar">
        <span v-if="enviando" class="spinner"></span>
        <span v-else>Participar en el Sorteo</span>
      </button>
    </main>

    <div v-if="enviado" class="modal-fondo" @click.self="enviado = false">
      <div class="modal">
        <div class="icono-exito">✅</div>
        <h2>¡Inscripción exitosa!</h2>
        <p>
          <strong>{{ campos.nombre }} {{ campos.apellido }}</strong
          >, su participación ha sido registrada.
        </p>
        <p class="modal-texto">
          El sistema corroborará que el formulario corresponde a personas con suscripción vigente a las revistas de Aula. Si no tienes suscripción vigente, en los próximos días te enviará la notificación de que no es válido al correo o celular que anotaste aquí. El sorteo es el día 31 de julio entre los formularios recibidos que corresponden a suscripciones vigentes. Gracias por participar.
        </p>
        <p class="aviso-prueba">Este es un sitio de pruebas. No se almacenaron datos reales.</p>
        <button class="btn-cerrar" @click="enviado = false">Cerrar</button>
      </div>
    </div>
  </div>
</template>

<style>
*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
  background: linear-gradient(135deg, #0f0c29 0%, #302b63 50%, #24243e 100%);
  color: #e0e0e0;
  min-height: 100vh;
}
</style>

<style scoped>
.contenedor {
  max-width: 700px;
  margin: 0 auto;
  padding: 1.5rem;
}

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

.subtitulo {
  margin-top: 0.4rem;
  font-size: 1.1rem;
  color: #b0b0c0;
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

.nota {
  margin-top: 0.6rem;
  font-size: 0.8rem;
  color: #777;
  font-style: italic;
}

.formulario-contenedor {
  background: rgba(255, 255, 255, 0.05);
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 16px;
  padding: 2rem;
  backdrop-filter: blur(10px);
}

.formulario-contenedor h2 {
  text-align: center;
  font-size: 1.3rem;
  margin-bottom: 1.5rem;
  color: #ccc;
}

.grid-campos {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.2rem;
}

.grupo-campo {
  display: flex;
  flex-direction: column;
}

.checkboxes {
  display: flex;
  flex-wrap: wrap;
  gap: 0.6rem;
  padding: 0.6rem 0.8rem;
  border-radius: 8px;
  border: 1px solid rgba(255, 255, 255, 0.15);
  background: rgba(255, 255, 255, 0.07);
  transition: border-color 0.2s, box-shadow 0.2s;
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

.checkbox-opcion input[type='checkbox'] {
  width: 16px;
  height: 16px;
  accent-color: #f7971e;
  cursor: pointer;
}

.grupo-checkboxes {
  grid-column: span 2;
}

label {
  font-size: 0.85rem;
  font-weight: 600;
  margin-bottom: 0.3rem;
  color: #aaa;
}

input,
select {
  padding: 0.65rem 0.8rem;
  border-radius: 8px;
  border: 1px solid rgba(255, 255, 255, 0.15);
  background: rgba(255, 255, 255, 0.07);
  color: #e0e0e0;
  font-size: 0.95rem;
  font-family: inherit;
  transition: border-color 0.2s, box-shadow 0.2s;
  outline: none;
}

input:focus,
select:focus {
  border-color: #f7971e;
  box-shadow: 0 0 0 3px rgba(247, 151, 30, 0.2);
}

input.error,
select.error {
  border-color: #e74c3c;
  box-shadow: 0 0 0 3px rgba(231, 76, 60, 0.15);
}

select {
  cursor: pointer;
}

select option {
  background: #1a1a2e;
  color: #e0e0e0;
}

.msg-error {
  font-size: 0.75rem;
  color: #e74c3c;
  margin-top: 0.25rem;
}

.requerido {
  color: #e74c3c;
}

.btn-enviar {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  margin-top: 1.8rem;
  padding: 0.85rem;
  font-size: 1.05rem;
  font-weight: 700;
  font-family: inherit;
  color: #1a1a2e;
  background: linear-gradient(90deg, #f7971e, #ffd200);
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: transform 0.15s, box-shadow 0.15s;
}

.error-servidor {
  margin-top: 1.2rem;
  padding: 0.7rem 1rem;
  background: rgba(231, 76, 60, 0.15);
  border: 1px solid rgba(231, 76, 60, 0.3);
  border-radius: 8px;
  color: #e74c3c;
  font-size: 0.85rem;
  text-align: center;
}

.btn-enviar:hover:not(:disabled) {
  transform: translateY(-1px);
  box-shadow: 0 6px 20px rgba(247, 151, 30, 0.35);
}

.btn-enviar:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

.spinner {
  width: 20px;
  height: 20px;
  border: 3px solid rgba(26, 26, 46, 0.3);
  border-top-color: #1a1a2e;
  border-radius: 50%;
  animation: girar 0.6s linear infinite;
}

@keyframes girar {
  to {
    transform: rotate(360deg);
  }
}

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

.icono-exito {
  font-size: 3rem;
  margin-bottom: 0.5rem;
}

.aviso-prueba {
  margin-top: 1rem;
  font-size: 0.8rem !important;
  color: #888 !important;
  font-style: italic;
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
  transition: transform 0.15s, box-shadow 0.15s;
}

.btn-cerrar:hover {
  transform: translateY(-1px);
  box-shadow: 0 6px 20px rgba(247, 151, 30, 0.35);
}

@media (max-width: 520px) {
  .grid-campos {
    grid-template-columns: 1fr;
  }

  .grupo-checkboxes {
    grid-column: span 1;
  }

  h1 {
    font-size: 1.7rem;
  }

  .formulario-contenedor {
    padding: 1.2rem;
  }
}
</style>
