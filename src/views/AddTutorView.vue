<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import { estados } from '@/services/global';

const API_URL = 'http://localhost:3000';

const route = useRouter();
const novoTutor = ref({
  nome: '',
  telefone: '',
  cnpj_cpf: '',
  email: '',
  data: '',
  endereco: {
    cep: '',
    rua: '',
    bairro: '',
    cidade: '',
    estado: '',
    numero: '',
  },
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
      <p class="text-body-secondary">Cadastre um novo tutor no sistema.</p>
    </header>
    <form
      v-on:submit.prevent="salvarTutor"
      class="row"
    >
      <h4>Dados do Tutor</h4>
      <section class="d-flex border-top border-2 gap-5">
        <div class="w-100">
          <label
            for="id"
            class="form-label"
          >
            ID
          </label>
          <input
            type="text"
            class="form-control bg-light"
            placeholder="Gerado automaticamente"
            disabled
          />
        </div>
        <div class="w-100">
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
            placeholder="Digite o nome completo"
            v-model="novoTutor.nome"
          />
        </div>
      </section>
      <section class="d-flex gap-5">
        <div class="w-100">
          <label
            for="cpf"
            class="form-label"
          >
            CPF / CNPJ
          </label>
          <input
            type="text"
            id="cpf"
            class="form-control"
            placeholder="Digite seu cpf..."
            v-model="novoTutor.cnpj_cpf"
          />
        </div>
        <div class="w-100">
          <label
            class="form-label"
            for="telefone"
          >
            Telefone/Whatsapp:
          </label>
          <input
            type="text"
            id="telefone"
            class="form-control"
            v-model="novoTutor.telefone"
          />
        </div>
      </section>

      <section class="d-flex gap-5">
        <div class="w-100">
          <label
            for="email"
            class="form-label"
          >
            E-mail:
          </label>
          <input
            type="email"
            id="email"
            class="form-control"
            placeholder="Digite seu email..."
            v-model="novoTutor.email"
          />
        </div>
        <div class="w-100">
          <label
            class="form-label"
            for="data"
          >
            Data de Nascimento:
          </label>
          <input
            type="date"
            id="data"
            class="form-control"
            v-model="novoTutor.data"
          />
        </div>
      </section>

      <h4 class="mt-3">Endereço</h4>
      <section class="d-flex border-top border-2 gap-5">
        <div class="w-100">
          <label
            for="CEP"
            class="form-label"
          >
            CEP:
          </label>
          <input
            type="text"
            id="cep"
            class="form-control"
            placeholder="Digite seu cep aqui"
            v-model="novoTutor.endereco.cep"
          />
        </div>
        <div class="w-50">
          <label
            class="form-label"
            for="num"
          >
            Número:
          </label>
          <input
            type="text"
            id="num"
            class="form-control"
            placeholder="Digite o seu número"
            v-model="novoTutor.endereco.numero"
          />
        </div>
      </section>
      <section class="d-flex gap-5">
        <div class="w-100">
          <label
            for="bairro"
            class="form-label"
          >
            Bairro:
          </label>
          <input
            type="text"
            id="bairro"
            class="form-control"
            placeholder="Digite seu bairro"
            v-model="novoTutor.endereco.bairro"
          />
        </div>
        <div class="w-100">
          <label
            class="form-label"
            for="bairro"
          >
            Cidade:
          </label>
          <input
            type="text"
            id="bairro"
            class="form-control"
            v-model="novoTutor.endereco.bairro"
          />
        </div>
      </section>

      <section class="d-flex gap-5">
        <div class="w-100">
          <label
            for="estado"
            class="form-label"
          >
            Estado:
          </label>
          <select
            id="estado"
            placeholder="Estado"
            class="form-select"
            v-model="novoTutor.endereco.estado"
          >
            <option
              v-for="uf in estados"
              :key="uf.uf"
              :value="uf.value"
            >
              {{ uf.uf }}
            </option>
          </select>
        </div>
        <div class="w-100">
          <label
            class="form-label"
            for="rua"
          >
            Rua:
          </label>
          <input
            type="text"
            id="rua"
            class="form-control"
            v-model="novoTutor.endereco.rua"
          />
        </div>
      </section>

      <div class="mt-4 gap-3 d-flex justify-content-end">
        <button class="btn btn-outline-dark">Cancelar</button>
        <button
          class="btn btn-primary"
          type="submit"
          >
          Cadastrar Tutor
        </button>
      </div>
    </form>
  </div>
</template>

<style scoped></style>
