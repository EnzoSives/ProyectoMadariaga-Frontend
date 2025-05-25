<template>
  <q-card class="q-pa-md">
    <q-form @submit.prevent="crearPersona" class="q-gutter-md">
      <div class="text-subtitle1 q-mb-sm">Datos Personales</div>
      <q-input filled v-model="form.nombre" label="Nombre" required />
      <q-input filled v-model="form.apellido" label="Apellido" required />
      <q-input filled v-model="form.dni" label="DNI" type="number" required />
      <q-select
        filled
        v-model="form.genero"
        :options="generos"
        label="Género"
        emit-value
        map-options
        required
      />
      <q-input
        filled
        v-model="form.fecha_nacimiento"
        label="Fecha de nacimiento"
        type="date"
        required
      />
      <q-input filled v-model="form.lugar_de_nacimiento" label="Lugar de nacimiento" required />
      <q-input filled v-model="form.lugar_de_residencia" label="Lugar de residencia" required />
      <q-input
        filled
        v-model="form.estadoCivil"
        label="Estado civil (número)"
        type="number"
        required
      />

      <q-separator />

      <div class="text-subtitle1 q-mt-md q-mb-sm">Contacto</div>
      <q-input filled v-model="form.email" label="Email" type="email" required />
      <q-input filled v-model="form.telefono" label="Teléfono" required />
      <q-input filled v-model="form.cuil" label="CUIL" required />
      <q-input filled v-model="form.domicilio" label="Domicilio" required />
      <q-input filled v-model="form.barrio" label="Barrio" required />

      <q-input
        filled
        v-model.number="form.vivienda"
        label="Vivienda (número)"
        type="number"
        required
      />

      <q-separator />

      <div class="text-subtitle1 q-mt-md q-mb-sm">Información Adicional</div>
      <q-toggle v-model="form.empleoActual" label="¿Tiene empleo actual?" />
      <q-input
        v-if="form.empleoActual"
        filled
        v-model="form.empleoActualData"
        label="Detalle del empleo"
      />

      <q-toggle v-model="form.escolaridad" label="¿Tiene escolaridad?" />
      <q-input
        v-if="form.escolaridad"
        filled
        v-model="form.escolaridadData"
        label="Detalle de escolaridad"
      />

      <q-toggle v-model="form.terciario" label="¿Tiene estudios terciarios?" />
      <q-input
        v-if="form.terciario"
        filled
        v-model="form.terciarioData"
        label="Detalle de terciario"
      />

      <q-toggle v-model="form.universitario" label="¿Universitario?" />
      <q-toggle v-model="form.curso" label="¿Ha hecho cursos?" />
      <q-toggle v-model="form.experiencia_laboral" label="¿Tiene experiencia laboral?" />

      <q-input filled v-model.number="form.id_Imagen" label="ID de Imagen" type="number" required />

      <div class="q-mt-lg">
        <q-btn label="Crear Persona" type="submit" color="primary" />
      </div>
    </q-form>
  </q-card>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import axios from 'axios';
import { useRouter } from 'vue-router';

const router = useRouter();

const form = ref({
  nombre: '',
  apellido: '',
  dni: '',
  genero: 1,
  fecha_nacimiento: '',
  lugar_de_nacimiento: '',
  lugar_de_residencia: '',
  estadoCivil: 1,
  cuil: '',
  domicilio: '',
  barrio: '',
  vivienda: 1,
  email: '',
  telefono: '',
  empleoActual: false,
  empleoActualData: '',
  escolaridad: false,
  escolaridadData: '',
  terciario: false,
  terciarioData: '',
  universitario: false,
  curso: false,
  experiencia_laboral: false,
  id_Imagen: 1,
});

const generos = [
  { label: 'Masculino', value: 1 },
  { label: 'Femenino', value: 2 },
  { label: 'Otro', value: 3 },
];

async function crearPersona() {
  try {
    const payload = { ...form.value };

    if (!payload.fecha_nacimiento || isNaN(new Date(payload.fecha_nacimiento).getTime())) {
      alert('La fecha de nacimiento no es válida');
      return;
    }

    // Asegurarse de que sea una fecha en formato YYYY-MM-DD
    const fecha = new Date(payload.fecha_nacimiento);
    payload.fecha_nacimiento = fecha.toISOString().split('T')[0]; // yyyy-mm-dd

    await axios.post('http://localhost:3000/persona', payload);
    await router.push('/');
  } catch (error) {
    console.error('Error al crear persona:', error);
  }
}
</script>
