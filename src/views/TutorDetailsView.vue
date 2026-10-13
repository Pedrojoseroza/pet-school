<script setup>
import { onMounted, ref } from 'vue';
import { useRoute } from 'vue-router';

const API_URL = 'http://localhost:3000';

const route = useRoute();
const tutor = ref({});
const pets = ref([]);

async function carregarTutorDetails() {
  const tutorId = route.params.id;
  const respostaTutor = await fetch(`${API_URL}/tutores/${tutorId}`);
  tutor.value = await respostaTutor.json();

  const respostaPet = await fetch(`${API_URL}/pets`);
  pets.value = await respostaPet.json();
  pets.value = pets.value.filter(e=> e.tutorId === tutorId);
}

onMounted(carregarTutorDetails);
</script>

<template>
  <div>
     <h1>Nome do Tutor: {{ tutor.nome }}</h1>
     <p>Telefone: {{ tutor.telefone }}</p>
    <p>
      Pets deste Tutor:
    </p>
    <p class="text text-danger" v-if="pets.length == 0">
      Este Tutor não possui nenhum pet!
    </p>
     <table class="table table-striped table-hover" v-else>
      <thead>
        <tr>
          <th>ID</th>
          <th>Nome</th>
          <th>Especie</th>
        </tr>
      </thead>

      <tbody>
        <tr
          v-for="pet in pets"
          :key="pet.id"
        >
          <td>{{ pet.id }}</td>
          <td>{{ pet.nome }}</td>
          <td>{{ pet.especie }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>

</style>
