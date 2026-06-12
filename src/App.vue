<script setup lang="ts">
import { ref, reactive } from "vue";
import AppHeader from "./components/AppHeader.vue";
import CampoFormulario from "./components/CampoFormulario.vue";
import CheckboxRevistas from "./components/CheckboxRevistas.vue";
import ModalExito from "./components/ModalExito.vue";
import ModalBases from "./components/ModalBases.vue";

const campos = reactive({
  nombre: "",
  apellido: "",
  celular: "",
  email: "",
  cedula: "",
  revistas: [] as string[],
});

const errores = reactive<Record<string, string>>({});
const enviado = ref(false);
const enviando = ref(false);
const mostrarBases = ref(false);

const opcionesRevistas = ["Primaria", "Niño en Obra", "Educación del Pueblo"];

const errorServidor = ref("");

const redirigirAula = () => {
  window.location.href = "https://aula.com.uy";
};

const validar = (): boolean => {
  Object.keys(errores).forEach((k) => delete errores[k]);
  errorServidor.value = "";

  if (!campos.nombre.trim()) errores.nombre = "Ingrese su nombre";
  if (!campos.apellido.trim()) errores.apellido = "Ingrese su apellido";
  if (!campos.celular.trim()) errores.celular = "Ingrese su celular";
  else if (!/^\d{7,15}$/.test(campos.celular.replace(/\D/g, "")))
    errores.celular = "Ingrese un número válido";
  if (!campos.email.trim()) errores.email = "Ingrese su email";
  else if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(campos.email))
    errores.email = "Ingrese un email válido";
  if (!campos.cedula.trim()) errores.cedula = "Ingrese su cédula";
  else if (!/^\d{8}$/.test(campos.cedula.replace(/\D/g, "")))
    errores.cedula = "La cédula debe tener 8 dígitos";
  if (campos.revistas.length === 0)
    errores.revistas = "Seleccione al menos una revista";

  return Object.keys(errores).length === 0;
};

const enviar = async () => {
  if (!validar()) return;
  enviando.value = true;

  try {
    const response = await fetch("https://aula.com.uy/larete/api/sorteo/2026", {
      method: "POST",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify({
        nombre: campos.nombre.trim(),
        apellido: campos.apellido.trim(),
        celular: campos.celular.trim(),
        email: campos.email.trim(),
        cedula: campos.cedula.replace(/[\s.-]/g, ""),
        revistas: campos.revistas,
      }),
    });

    if (!response.ok) {
      const data = await response.json();
      if (data.errors) {
        for (const [campo, mensajes] of Object.entries(data.errors)) {
          errores[campo] = Array.isArray(mensajes)
            ? (mensajes as string[])[0]
            : String(mensajes);
        }
      }
      errorServidor.value = data.message || "Error al enviar el formulario.";
      return;
    }

    enviado.value = true;
  } catch (err) {
    if (err instanceof TypeError && err.message === "Failed to fetch") {
      console.log(
        "Error de red: no se pudo alcanzar el servidor (posible CORS, DNS, o servidor caído).",
        err,
      );
    } else if (err instanceof Error) {
      console.log("Error inesperado al enviar formulario:", err.message, err);
    } else {
      console.log("Error desconocido al enviar formulario:", err);
    }
    errorServidor.value =
      "Error de conexión. Verifique su internet e intente nuevamente.";
  } finally {
    enviando.value = false;
  }
};
</script>

<template>
  <div class="contenedor">
    <AppHeader />

    <main class="formulario-contenedor" v-if="!enviado">
      <h2>Formulario de Participación</h2>

      <div class="grid-campos">
        <CampoFormulario
          id="nombre"
          label="Nombre"
          v-model="campos.nombre"
          :error="errores.nombre"
        />
        <CampoFormulario
          id="apellido"
          label="Apellido"
          v-model="campos.apellido"
          :error="errores.apellido"
        />
        <CampoFormulario
          id="celular"
          label="Celular"
          type="tel"
          v-model="campos.celular"
          :error="errores.celular"
        />
        <CampoFormulario
          id="email"
          label="Email"
          type="email"
          v-model="campos.email"
          :error="errores.email"
        />
        <CampoFormulario
          id="cedula"
          label="Cédula"
          v-model="campos.cedula"
          :error="errores.cedula"
        />
        <CheckboxRevistas
          :opciones="opcionesRevistas"
          v-model="campos.revistas"
          :error="errores.revistas"
        />
      </div>

      <div v-if="errorServidor" class="error-servidor">{{ errorServidor }}</div>

      <button class="btn-enviar" :disabled="enviando" @click="enviar">
        <span v-if="enviando" class="spinner"></span>
        <span v-else>Participar en el Sorteo</span>
      </button>

      <button class="btn-bases" @click="mostrarBases = true">Bases y condiciones</button>
    </main>

    <ModalExito
      v-if="enviado"
      :nombre="campos.nombre"
      :apellido="campos.apellido"
      @cerrar="redirigirAula"
    />

    <ModalBases
      v-if="mostrarBases"
      @cerrar="mostrarBases = false"
    />
  </div>
</template>

<style scoped>
.contenedor {
  max-width: 700px;
  margin: 0 auto;
  padding: 1.5rem;
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
  transition:
    transform 0.15s,
    box-shadow 0.15s;
}

.btn-enviar:hover:not(:disabled) {
  transform: translateY(-1px);
  box-shadow: 0 6px 20px rgba(247, 151, 30, 0.35);
}

.btn-enviar:disabled {
  opacity: 0.7;
  cursor: not-allowed;
}

.btn-bases {
  display: block;
  margin: 0.8rem auto 0 auto;
  padding: 0;
  font-size: 0.8rem;
  font-family: inherit;
  color: #999;
  background: none;
  border: none;
  cursor: pointer;
  text-decoration: underline;
  text-underline-offset: 2px;
  transition: color 0.15s;
}

.btn-bases:hover {
  color: #f7971e;
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

@media (max-width: 520px) {
  .grid-campos {
    grid-template-columns: 1fr;
  }

  .formulario-contenedor {
    padding: 1.2rem;
  }
}
</style>
