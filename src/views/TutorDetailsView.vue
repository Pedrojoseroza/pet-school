<script setup>
import { onMounted, ref } from 'vue';
import { useRoute, useRouter } from 'vue-router';

const API_URL = 'http://localhost:3000';

const router = useRouter();
const route = useRoute();
const tutor = ref({});
const pets = ref([]);
const loading = ref(true);
async function carregarTutorDetails() {
  const tutorId = route.params.id;
  const respostaTutor = await fetch(`${API_URL}/tutores/${tutorId}`);
  tutor.value = await respostaTutor.json();
  console.log(tutor.value.endereco['rua']);

  const respostaPet = await fetch(`${API_URL}/pets`);
  pets.value = await respostaPet.json();
  pets.value = pets.value.filter((e) => e.tutorId === tutorId);
  loading.value = false;
}


onMounted(carregarTutorDetails);
</script>

<template>
  <div v-if="loading">
    <h1 >
      Dados Carregando...
    </h1>
  </div>
  <div v-else>
    <section class="d-flex align-items-center justify-content-between">
      <div class="">
        <h1>Dados do Tutor</h1>
        <p>Visualize os dados dos seus tutores e de seus pets vinculados</p>
      </div>
      <button
        class="btn btn-outline-primary fs-3"
        @click="router.back()"
      >
        <- Voltar
      </button>
    </section>
    <section class="card shadow-sm mb-4">
      <div class="card-header bg-light">
        <h2 class="h5 mb-0">Dados pessoais</h2>
      </div>
      <div class="card-body">
        <div class="row g-3 row-cols-3">
          <div>
            <strong>ID</strong>
            <p>
              {{ tutor.id }}
            </p>
          </div>
          <div>
            <strong>Nome Completo</strong>
            <p>
              {{ tutor.nome }}
            </p>
          </div>

          <div>
            <strong>CPF/CNPJ</strong>
            <p>
              {{ tutor.cnpj_cpf }}
            </p>
          </div>

          <div>
            <strong>Telefone/Celular/Whatsapp</strong>
            <p>
              {{ tutor.telefone }}
            </p>
          </div>

          <div>
            <strong>Email</strong>
            <p>
              {{ tutor.email }}
            </p>
          </div>

          <div>
            <strong>Data de Nascimento</strong>
            <p>
              {{ new Date(tutor.data).toLocaleDateString('pt-BR') }}
            </p>
          </div>
        </div>
      </div>
    </section>

    <section class="card shadow-sm mb-4">
      <div class="card-header bg-light">
        <h2 class="h5 mb-0">Endereço</h2>
      </div>
      <div class="card-body">
        <div class="row g-3 row-cols-3">
          <div>
            <strong>Rua</strong>
            <p>
              {{ tutor.endereco.rua }}
            </p>
          </div>
          <div>
            <strong>Número</strong>
            <p>
              {{ tutor.endereco.numero }}
            </p>
          </div>

          <div>
            <strong>Bairro</strong>
            <p>
              {{ tutor.endereco.bairro }}
            </p>
          </div>

          <div>
            <strong>Cidade</strong>
            <p>
              {{ tutor.endereco.cidade }}
            </p>
          </div>

          <div>
            <strong>Estado</strong>
            <p>
              {{ tutor.endereco.estado }}
            </p>
          </div>

          <div>
            <strong>CEP</strong>
            <p>
              {{ tutor.endereco.cep }}
            </p>
          </div>
        </div>
      </div>
    </section>
    <h3>Pets deste Tutor:</h3>
    <p
      class="text text-danger"
      v-if="pets.length == 0"
    >
      Este Tutor não possui nenhum pet!
    </p>
    <table
      class="table table-striped table-hover"
      v-else
    >
      <thead>
        <tr>
          <th>ID</th>
          <th>Nome</th>
          <th>Especie</th>
          <th>Ações</th>
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
          <td>
            <RouterLink
              :to="`/pets/${pet.id}`"
              class="btn btn-primary"
            >
              Visualizar
            </RouterLink>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped></style>
