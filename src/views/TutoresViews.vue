<script setup>
import { onMounted, ref } from 'vue';

const API_URL = 'http://localhost:3000';

const tutores = ref([]);
async function carregarTutores() {
  const respostaTutores = await fetch(`${API_URL}/tutores`);
  tutores.value = await respostaTutores.json();
}


async function excluirTutor(id) {
  const certezaExclusao = window.confirm('Deseja realmente excluir o tutor?');
  if (certezaExclusao) {
    await fetch(`${API_URL}/tutores/${id}`, {
      method: 'DELETE',
    });
  }
  await carregarTutores(); //Carrega os dados após a exclusão
}

onMounted(carregarTutores);

</script>

<template>
  <div>
    <header class="mb-4">
      <h1 class="text-2xl font-bold">Listagem dos Tutores</h1>
      <p class="text-body-secondary mb-0">
        Listagem dos Tutores de Pets cadastrados no sistema.
      </p>
    </header>

    <RouterLink
      class="btn btn-primary"
      :to="{ name: 'addTutor' }"
    >
      Adicionar Tutor
    </RouterLink>

    <table class="table table-striped table-hover">
      <thead>
        <tr>
          <th>ID</th>
          <th>Nome</th>
          <th>Número</th>
        </tr>
      </thead>

      <tbody>
        <tr
          v-for="tutor in tutores"
          :key="tutor.id"
        >
          <td>
            {{ tutor.id }}
          </td>
          <td>
            {{ tutor.nome }}
          </td>
          <td>
            {{ tutor.telefone }}
          </td>
          <td>
            {{tutor.email}}
          </td>
          <td>
            
          </td>
          <td class="d-flex align-items-center">
            <RouterLink :to="`/tutores/${tutor.id}`" class="btn btn-primary"> Visualizar </RouterLink>
            <button class="btn btn-danger ms-3 p-1" @click="excluirTutor(tutor.id)">Deletar</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped></style>
