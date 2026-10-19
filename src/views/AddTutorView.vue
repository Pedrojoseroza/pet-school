<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';

const API_URL = 'http://localhost:3000';

const route = useRouter();
const novoTutor = ref({
  nome: '',
  telefone: '',
});

async function salvarTutor() {
  if (novoTutor.value.nome.trim() !== '') {
    await fetch(`${API_URL}/tutores`, {
      method: 'POST',
      header: {
        'Content-type': 'application/json',
      },
      body: JSON.stringify(novoTutor.value),
    });
    route.push('/tutores');
  }
}
</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Cadastro de Tutores</h1>
      <p class="text-body-secondary mb-0">Cadastre um novo tutor no sistema.</p>
    </header>
    <form v-on:submit.prevent="salvarTutor" class="form form-bordered min-w-100">
      <div class="col-md-6">
        <div class="row p-2">
          <div class="col">
            <label for="id" class="form-label">
              ID
            </label>
            <input
            type="text"
            class="form-control bg-light"
            placeholder="Gerado automaticamente"
            disabled
            >
          </div>
          <div class="col">
            <label
              class="form-label"
              for="nome"
            >
              Nome:
            </label>
            <input
              type="text"
              id="nome"
              class="form-control"
              v-model="novoTutor.nome"
            />
          </div>
        </div>

        <label
          class="form-label"
          for="num"
        >
          Telefone:
        </label>
        <input
          type="number"
          id="num"
          class="form-control"
          v-model="novoTutor.telefone"
        />
      </div>
    </form>
  </div>
</template>

<style scoped></style>
