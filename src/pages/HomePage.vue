<template>
  <q-page padding>
    <q-row class="q-gutter-md">
      <q-col cols="12" md="6">
        <h1 class="text-h4">Bienvenido a la Aplicación</h1>
        <p class="text-body1">
          Esta es una aplicación de ejemplo para mostrar datos de personas y empresas.
        </p>

        <!-- Tabs -->
        <q-tabs v-model="tab" class="text-primary" dense align="left" narrow-indicator>
          <q-tab name="personas" label="Personas" />
          <q-tab name="empresas" label="Empresas" />
        </q-tabs>

        <q-separator />

        <!-- Paneles -->
        <q-tab-panels v-model="tab" animated>
          <!-- Panel de Personas -->
          <q-tab-panel name="personas">
            <div class="row items-center justify-between q-mb-md">
              <h2 class="text-h5">Últimas Personas</h2>
              <q-btn
                color="primary"
                icon="person_add"
                label="Agregar Persona"
                @click="irACrearPersona"
              />
            </div>
            <q-card v-for="persona in personas" :key="persona.id_persona" class="q-mb-md">
              <q-card-section>
                <div class="text-h6">{{ persona.nombre }} {{ persona.apellido }}</div>
                <div class="text-subtitle2">DNI: {{ persona.dni }}</div>
                <div class="text-caption">Fecha de nacimiento: {{ persona.fecha_nacimiento }}</div>
                <div class="text-caption">Residencia: {{ persona.lugar_de_residencia }}</div>
                <div v-if="persona.empleoActual" class="text-body2 q-mt-sm">
                  💼 {{ persona.empleoActualData }}
                </div>
                <div v-if="persona.escolaridad" class="text-caption">
                  🎓 {{ persona.escolaridadData }}
                </div>
              </q-card-section>
            </q-card>
          </q-tab-panel>

          <!-- Panel de Empresas -->
          <q-tab-panel name="empresas">
            <h2 class="text-h5">Últimas Empresas</h2>
            <q-card v-for="empresa in empresas" :key="empresa.id_empresa" class="q-mb-md">
              <q-card-section>
                <div class="text-h6">{{ empresa.nombre }}</div>
                <div class="text-subtitle2">CUIT: {{ empresa.cuit }}</div>
                <div class="text-caption">Dirección: {{ empresa.direccion }}</div>
                <div class="text-caption">Email: {{ empresa.email }}</div>
                <div class="text-caption">Teléfono: {{ empresa.telefono }}</div>
              </q-card-section>
            </q-card>
          </q-tab-panel>
        </q-tab-panels>
      </q-col>
    </q-row>
  </q-page>
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';

const router = useRouter();

async function irACrearPersona() {
  await router.push('/personas/crear');
}

const tab = ref('personas');
const personas = ref([]);
const empresas = ref([]);

onMounted(async () => {
  try {
    const resPersonas = await axios.get('http://localhost:3000/persona');
    personas.value = resPersonas.data.slice(-5).reverse();

    const resEmpresas = await axios.get('http://localhost:3000/empresa');
    empresas.value = resEmpresas.data.slice(-5).reverse();
  } catch (error) {
    console.error('Error al obtener datos:', error);
  }
});
</script>
