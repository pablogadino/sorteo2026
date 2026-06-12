<script setup lang="ts">
defineEmits<{
  cerrar: [];
}>();
</script>

<template>
  <div class="modal-fondo" @click.self="$emit('cerrar')">
    <div class="modal">
      <div class="modal-header">
        <h2>BASES Y CONDICIONES DEL SORTEO</h2>
        <button class="btn-cerrar-x" @click="$emit('cerrar')">&times;</button>
      </div>
      <div class="modal-body">
        <p class="subtitulo">
          Sorteo: Smart TV 50" — Ediciones Aula — 10 de julio de 2026
        </p>

        <section>
          <h3>1. ORGANIZADOR</h3>
          <p>
            El presente sorteo es organizado por Ediciones Aula (en adelante,
            "la Organizadora"), con domicilio en la República Oriental del
            Uruguay. La participación en el sorteo implica la aceptación plena e
            incondicional de las presentes Bases y Condiciones.
          </p>
        </section>

        <section>
          <h3>2. PERÍODO DE PARTICIPACIÓN</h3>
          <p>
            El período de inscripción se extiende desde la publicación del
            presente sorteo hasta el día 9 de julio de 2026 a las 23:59 horas
            (hora oficial de Uruguay, UTC-3). No se aceptarán inscripciones
            fuera de dicho plazo.
          </p>
        </section>

        <section>
          <h3>3. MECÁNICA DE PARTICIPACIÓN</h3>
          <p>
            Podrán participar todas las personas físicas que: (a) sean
            suscriptores activos de revista Primaria y/o Niño en Obra al momento
            de la inscripción; (b) sean mayores de 18 años de edad; (c) residan
            en la República Oriental del Uruguay; y (d) completen íntegramente
            el formulario oficial de participación habilitado por la
            Organizadora, consignando nombre completo, documento de identidad,
            correo electrónico y teléfono de contacto. Cada suscriptor podrá
            inscribirse una única vez. Las inscripciones duplicadas serán
            descartadas.
          </p>
        </section>

        <section>
          <h3>4. PREMIO</h3>
          <p>
            El premio consiste en un (1) televisor Smart TV de 50 pulgadas (o de
            características equivalentes en caso de falta de stock del modelo
            publicitado), sin posibilidad de canje por dinero en efectivo ni por
            otros productos o servicios. El premio no incluye gastos de traslado
            si el ganador reside fuera del área de entrega habitual de la
            Organizadora, salvo acuerdo expreso entre las partes.
          </p>
        </section>

        <section>
          <h3>5. REALIZACIÓN DEL SORTEO</h3>
          <p>
            El sorteo se realizará el día 10 de julio de 2026, mediante un
            sistema aleatorio informático o por el mecanismo público que la
            Organizadora determine, garantizando transparencia e imparcialidad.
            El resultado del sorteo tendrá carácter definitivo e inapelable.
          </p>
        </section>

        <section>
          <h3>6. NOTIFICACIÓN AL GANADOR</h3>
          <p>
            El ganador será notificado mediante el correo electrónico declarado
            en el formulario de inscripción dentro de las 48 horas hábiles
            posteriores al sorteo. Si el ganador no respondiera la notificación
            dentro de los 5 (cinco) días hábiles siguientes, la Organizadora
            podrá declarar desierto el premio o realizar un nuevo sorteo entre
            los participantes, a su exclusiva discreción.
          </p>
        </section>

        <section>
          <h3>7. ENTREGA DEL PREMIO</h3>
          <p>
            La entrega del premio se coordinará con el ganador y se efectuará
            dentro de los 30 (treinta) días corridos desde la confirmación de
            recepción de la notificación. El ganador deberá acreditar su
            identidad mediante documento de identidad vigente y su condición de
            suscriptor activo al momento del sorteo.
          </p>
        </section>

        <section>
          <h3>8. RESTRICCIONES</h3>
          <p>
            Quedan excluidos de la participación los empleados, colaboradores y
            directivos de Ediciones Aula, así como sus familiares directos
            (cónyuge, hijos, padres y hermanos). La Organizadora se reserva el
            derecho de verificar la elegibilidad de los participantes y de
            descalificar a quienes hayan proporcionado datos falsos o
            incompletos.
          </p>
        </section>

        <section>
          <h3>9. PROTECCIÓN DE DATOS PERSONALES</h3>
          <p>
            Los datos personales recopilados mediante el formulario de
            participación serán tratados conforme a lo establecido en la Ley N.°
            18.331 de Protección de Datos Personales de la República Oriental
            del Uruguay y su normativa complementaria. Los datos serán
            utilizados exclusivamente para la gestión del presente sorteo y, con
            el consentimiento del participante, para el envío de comunicaciones
            de Ediciones Aula. Los participantes podrán ejercer sus derechos de
            acceso, rectificación, actualización, inclusión o supresión
            dirigiéndose a la Organizadora por los canales oficiales de
            contacto.
          </p>
        </section>

        <section>
          <h3>10. RESPONSABILIDADES Y EXENCIONES</h3>
          <p>
            La Organizadora no será responsable por fallas técnicas, de
            conectividad o de cualquier otra naturaleza que impidan la recepción
            o el procesamiento de las inscripciones. Asimismo, no asumirá
            responsabilidad por defectos de fabricación del premio, los cuales
            quedan sujetos a la garantía legal correspondiente y a las
            condiciones del fabricante.
          </p>
        </section>

        <section>
          <h3>11. MODIFICACIONES Y CANCELACIÓN</h3>
          <p>
            La Organizadora se reserva el derecho de modificar, suspender o
            cancelar el sorteo en cualquier momento por causas de fuerza mayor,
            caso fortuito u otras circunstancias debidamente justificadas, sin
            que ello genere derecho a compensación alguna a favor de los
            participantes.
          </p>
        </section>

        <section>
          <h3>12. LEGISLACIÓN APLICABLE Y JURISDICCIÓN</h3>
          <p>
            Las presentes Bases y Condiciones se rigen por las leyes de la
            República Oriental del Uruguay. Para cualquier controversia derivada
            de la interpretación o aplicación de las presentes bases, las partes
            se someten a la jurisdicción de los Tribunales competentes de la
            ciudad de Montevideo, renunciando a cualquier otro fuero o
            jurisdicción que pudiera corresponderles.
          </p>
        </section>

        <section>
          <h3>13. ACEPTACIÓN</h3>
          <p>
            La participación en el sorteo implica el conocimiento y la
            aceptación plena e incondicional de todas las cláusulas de las
            presentes Bases y Condiciones.
          </p>
        </section>
      </div>
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
  text-align: left;
  background: #1a1a2e;
  border: 1px solid rgba(255, 255, 255, 0.15);
  border-radius: 16px;
  padding: 0;
  max-width: 680px;
  width: 100%;
  max-height: 85vh;
  display: flex;
  flex-direction: column;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
}

.modal-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 1.5rem 2rem 0 2rem;
}

.modal-header h2 {
  font-size: 1.25rem;
  color: #f7971e;
  margin: 0;
  line-height: 1.3;
}

.btn-cerrar-x {
  background: none;
  border: none;
  color: #999;
  font-size: 1.8rem;
  cursor: pointer;
  line-height: 1;
  padding: 0;
  margin-left: 1rem;
  transition: color 0.15s;
}

.btn-cerrar-x:hover {
  color: #fff;
}

.modal-body {
  padding: 1rem 2rem 1.5rem 2rem;
  overflow-y: auto;
  flex: 1;
}

.subtitulo {
  font-size: 0.9rem;
  color: #aaa;
  margin-bottom: 1.2rem;
  font-style: italic;
}

section {
  margin-bottom: 1rem;
}

section h3 {
  font-size: 0.85rem;
  color: #f7971e;
  margin-bottom: 0.3rem;
}

section p {
  font-size: 0.82rem;
  color: #ccc;
  line-height: 1.55;
  margin: 0;
}

.btn-cerrar {
  display: block;
  width: calc(100% - 4rem);
  margin: 0 2rem 1.5rem 2rem;
  padding: 0.65rem;
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

@media (max-width: 520px) {
  .modal {
    max-height: 90vh;
  }

  .modal-header {
    padding: 1.2rem 1.2rem 0 1.2rem;
  }

  .modal-header h2 {
    font-size: 1.05rem;
  }

  .modal-body {
    padding: 0.8rem 1.2rem 1rem 1.2rem;
  }

  .btn-cerrar {
    width: calc(100% - 2.4rem);
    margin: 0 1.2rem 1.2rem 1.2rem;
  }
}
</style>
