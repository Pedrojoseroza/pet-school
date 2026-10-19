<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';

const API_URL = 'http://localhost:3000';

const route = useRouter();
const novoTutor = ref({
  nome: '',
  telefone: '',
  cnpj_cpf: ''
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
    <form v-on:submit.prevent="salvarTutor" class="row border card py-3">
      <h2>
        Dados do Tutor
      </h2>
      <section class="d-flex gap-5">
        <div class="w-100">
          <label for="id" class="form-label">
            ID
          </label>
          <input type="text" class="form-control bg-light" placeholder="Gerado automaticamente" disabled>
        </div>
        <div class="w-100">
          <label class="form-label" for="nome">
            Nome:
          </label>
          <input type="text" id="nome" class="form-control" placeholder="Digite o nome completo" v-model="novoTutor.nome" />
        </div>
      </section>
      <section class="d-flex gap-5">
        <div class="w-100">
          <label for="cpf" class="form-label">
            CPF / CNPJ
          </label>
          <input type="text" class="form-control" placeholder="Gerado automaticamente" disabled>
        </div>
        <div class="w-100">
          <label class="form-label" for="nome">
            Nome:
          </label>
          <input type="text" id="nome" class="form-control" v-model="novoTutor.nome" />
        </div>
      </section>

    </form>
  </div>
</template>

<style scoped></style>
