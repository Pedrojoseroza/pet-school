<script setup>
import { onMounted, ref } from 'vue';
import { RouterLink, useRouter } from 'vue-router';

const router = useRouter();

const novoPet = ref({
  nome: '',
  especie: '',
  tutorId: '',
});

// Aqui estou declarando a URL da API
const API_URL = 'http://localhost:3000';

const tutores = ref({});

async function getTutores() {
  const reposta = await fetch(`${API_URL}/tutores`);
  tutores.value = await reposta.json();
  console.log('load tutores', tutores);
}

async function postPet() {
  await fetch(`${API_URL}/pets`, {
    method: '',
    header: {
      'Content-type': 'application/json',
    },
    body: JSON.stringify(novoPet.value),
  });
  router.push('/pets');
}

onMounted(getTutores());
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem de Pets</h1>
      <p class="text-body-secondary mb-0">Cadastro de Pets no sistema.</p>
    </header>
    <p v-if="getTutores">
      Carregando tutores...
    </p>
    <p
    v-else
    class="alert alert-danger"
    role="alert">
    Deu B.O
  </p>
    <RouterLink
      class="btn btn-primary"
      :to="{ name: 'addPet' }"
    >
      Adicionar Pet
    </RouterLink>
    <form @submit.prevent="postPet">
      <div class="col-md-6">
        <label
          for="nome"
          class="form-label"
        >
          Nome do Pet
        </label>
        <input
          type="text"
          id="nome"
          class="form-control"
          v-model="novoPet.nome"
          required
        />
      </div>
      <div class="col-md-6">
        <label
          for="especie"
          class="form-label"
        >
          Espécie
        </label>

        <select
          id="especie"
          v-model="novoPet.especie"
          class="form-select"
          required
        >
          <option
            value=""
            disabled
          >
            Selecione espécie
          </option>
          <option value="cachorro">Cachorro</option>
          <option value="gato">Gato</option>
          <option value="coelho">Coelho</option>
          <option value="cobra">Serpente</option>
        </select>
      </div>

      <div class="col-md-6">
        <label
          for="tutor"
          class="form-label"
        >
          Tutor
        </label>

        <select
          id="tutor"
          v-model="novoPet.tutorId"
          class="form-select"
          required
        >
          <option
            value=""
            disabled
          >
            Selecione os tutores
          </option>
          <option class="form-option" v-for="tutor in tutores" :key="tutor.id" :value="tutor.id">
            {{ tutor.nome }}
          </option>
        </select>
      </div>

    </form>
  </div>
</template>
