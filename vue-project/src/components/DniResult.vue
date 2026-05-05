<!-- components/DniResult.vue — Tarjeta de resultado -->
<template>
  <div class="dni-result">

    <!-- Sin búsqueda aún -->
    <div v-if="estado === 'idle'" class="estado-vacio">
      <div class="icono">🪪</div>
      <p>Ingresa un DNI para consultar</p>
    </div>

    <!-- Cargando -->
    <div v-else-if="estado === 'cargando'" class="estado-vacio">
      <div class="icono">⏳</div>
      <p>Consultando...</p>
    </div>

    <!-- Éxito -->
    <div v-else-if="estado === 'exito'" class="estado-exito">
      <div class="tag-ok">
        <span class="dot-verde"></span>
        Registro encontrado
      </div>
      <div class="fila-dato">
        <span class="label">DNI</span>
        <span class="valor">{{ datos.dni || dniBuscado }}</span>
      </div>
      <div class="fila-dato">
        <span class="label">Nombre</span>
        <span class="valor">{{ datos.nombre || datos.name || '—' }}</span>
      </div>
      <div v-if="datos.apellidos || datos.apellido" class="fila-dato">
        <span class="label">Apellidos</span>
        <span class="valor">{{ datos.apellidos || datos.apellido }}</span>
      </div>
    </div>

    <!-- Error -->
    <div v-else-if="estado === 'error'" class="estado-error">
      <span class="icono-error">✕</span>
      <span>{{ mensajeError }}</span>
    </div>

  </div>
</template>

<script setup>
defineProps({
  estado:        { type: String, default: 'idle' },  // idle | cargando | exito | error
  datos:         { type: Object, default: () => ({}) },
  dniBuscado:    { type: String, default: '' },
  mensajeError:  { type: String, default: 'Error al consultar.' }
})
</script>