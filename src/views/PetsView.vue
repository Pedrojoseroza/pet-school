<script setup>
import { onMounted, ref } from 'vue';
const API_URL = 'http://localhost:3000';
const pets = ref([]);
const tutores = ref([]);
const loading = ref(true);

async function carregarDados() {
  const respostaPet = await fetch(`${API_URL}/pets`);
  pets.value = await respostaPet.json();

  const respostaTutores = await fetch(`${API_URL}/tutores`);
  tutores.value = await respostaTutores.json();
  loading.value = false;
  }
   const getTutor = (id) => {
    const tutor = tutores.value.find(e => e.id == id);
    console.log(tutor);
    if (tutor === '') {
      return "Deu B.O!";
    }
    return tutor.nome;
  }
onMounted(carregarDados);
</script>
<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de Pets</h1>
      <p class="text-body-secondary mb-0">
        Listagem dos Pets cadastrados no sistema.
      </p>
    </header>
    <main>
      <div class="container-fluid">
        <table class="table table-striped table-hover">
          <thead>
            <tr>
              <th>
              Identificador
            </th>
            <th>
              Nome
            </th>
            <th>
              Espécie
            </th>
            <th>
              Tutor
            </th>
            </tr>
          </thead>
          <tr v-for="pet in pets">
            <td>
              {{ pet.id }}
            </td>
            <td>
              {{ pet.nome }}
            </td>
            <td>
              {{ pet.especie }}
            </td>
            <td>
              {{ getTutor(pet.tutorId)}}
            </td>
          </tr>
        </table>
      </div>
    </main>
    <RouterLink
      class="btn btn-primary"
      :to="{ name: 'addPet' }"
    >
      Adicionar Pet
    </RouterLink>
  </div>
</template>
