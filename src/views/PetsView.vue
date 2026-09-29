<script setup>
import { onMounted, ref } from 'vue';

const API_URL = 'http://localhost:3000';

const pets = ref([]);
const tutores = ref([]);
const carregando = ref(true);
const erro = ref('');

async function carregarPets() {
  try {
    const resposta = await fetch(`${API_URL}/pets`);

    if (!resposta.ok) {
      throw new Error(`O servidor respondeu com o status ${resposta.status}.`);
    }

    pets.value = await resposta.json();
  } catch (falha) {
    erro.value = 'Não foi possível carregar os pets. Tente novamente.';
    console.error(falha);
  } finally {
    carregando.value = false;
  }
}

async function carregarTutores() {
  try {
    const resposta = await fetch(`${API_URL}/tutores`);

    if (!resposta.ok) {
      throw new Error(`O servidor respondeu com o status ${resposta.status}.`);
    }

    tutores.value = await resposta.json();
  } catch (falha) {
    console.error(falha);
  }
}

function nomeDoTutor(tutorId) {
  const tutor = tutores.value.find(
    (item) => String(item.id) === String(tutorId),
  );

  return tutor?.nome ?? 'Sem tutor';
}

onMounted(() => {
  carregarPets();
  carregarTutores();
});
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
      <p
        v-if="carregando"
        class="text-body-secondary"
        role="status"
      >
        Carregando pets...
      </p>

      <div
        v-else-if="erro"
        class="alert alert-danger"
        role="alert"
      >
        {{ erro }}
      </div>

      <p
        v-else-if="pets.length === 0"
        class="text-body-secondary"
      >
        Nenhum pet cadastrado.
      </p>

      <div
        v-else
        class="container-fluid"
      >
        <table class="table table-striped table-hover">
          <thead>
            <tr>
              <th>Identificador</th>
              <th>Nome</th>
              <th>Espécie</th>
              <th>Tutor</th>
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
              <td>{{ nomeDoTutor(pet.tutorId) }}</td>
            </tr>
          </tbody>
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
