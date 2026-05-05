<!-- views/ConsultaDni.vue  ← con Router
     App.vue               ← sin Router  -->
<template>
  <div class="dni-page">

    <!-- Encabezado -->
    <div class="dni-header">
      <span class="eyebrow">API Cloud · Perú</span>
      <h1>Consulta DNI</h1>
      <p>Obtén el nombre completo a partir del número de documento.</p>
    </div>

    <!-- Card -->
    <div class="dni-card">

      <!-- Input + botón -->
      <div class="input-group">
        <input
          v-model="dni"
          class="dni-input"
          type="text"
          placeholder="00000000"
          maxlength="8"
          @input="dni = dni.replace(/\D/g, '')"
          @keyup.enter="consultar"
        />
        <button class="btn-buscar" @click="consultar" :disabled="estado === 'cargando'">
          <span v-if="estado === 'cargando'" class="spinner"></span>
          <span>{{ estado === 'cargando' ? 'Buscando' : 'Buscar' }}</span>
        </button>
      </div>
      <p class="hint">8 dígitos · solo números</p>

      <div class="divider"></div>

      <!-- Componente resultado -->
      <DniResult
        :estado="estado"
        :datos="resultado"
        :dni-buscado="dni"
        :mensaje-error="error"
      />

    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import axios from 'axios'
import DniResult from '@/components/DniResult.vue'
import '@/assets/styles/dni.css'

// ── EDITA AQUÍ ──────────────────────────────
const API_URL   = 'https://TU-API.cloud/v1/dni'
const API_TOKEN = 'TU_TOKEN_AQUI'
// ────────────────────────────────────────────

const dni      = ref('')
const estado   = ref('idle')   // idle | cargando | exito | error
const resultado = ref({})
const error    = ref('')

async function consultar() {
  if (dni.value.length !== 8) {
    estado.value = 'error'
    error.value  = 'El DNI debe tener exactamente 8 dígitos.'
    return
  }

  estado.value   = 'cargando'
  resultado.value = {}

  try {
    const { data } = await axios.get(`${API_URL}?dni=${dni.value}`, {
      headers: {
        Authorization: `Bearer ${API_TOKEN}`,
        Accept: 'application/json'
      }
    })
    resultado.value = data
    estado.value    = 'exito'
  } catch (err) {
    error.value  = err.response?.data?.message || 'DNI no encontrado o error de conexión.'
    estado.value = 'error'
  }
}
</script
