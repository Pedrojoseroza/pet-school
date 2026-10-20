<script setup>
import { ref } from 'vue';
import { useRouter } from 'vue-router';
import { estados } from '@/services/global';

const API_URL = 'http://localhost:3000';

const route = useRouter();
const estruturaTutor = {
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
};

const criaTutorVazio = () => ({
  ...estruturaTutor,
  endereco: { ...estruturaTutor.endereco },
});

const novoTutor = ref(criaTutorVazio());
const erro = ref(criaTutorVazio());

const validaForm = () => {
  erro.value = criaTutorVazio();
  let eValido = true;

  if (novoTutor.value.nome.trim() === '') {
    eValido = false;
    erro.value.nome = 'Este campo está vazio, preenche-o!';
  }
  if (novoTutor.value.cnpj_cpf.trim() === '') {
    eValido = false;
    erro.value.cnpj_cpf = 'Este campo está vazio, preenche-o!';
  }
  if (novoTutor.value.telefone.trim() === '') {
    eValido = false;
    erro.value.telefone = 'Este campo está vazio, preenche-o!';
  }
  if (novoTutor.value.email.trim() === '') {
    eValido = false;
    erro.value.email = 'Este campo está vazio, preenche-o!';
  }
  if (novoTutor.value.data.trim() === '') {
    eValido = false;
    erro.value.data = 'Este campo está vazio, preenche-o!';
  }
  if (novoTutor.value.endereco.cep.trim() === '') {
    eValido = false;
    erro.value.endereco.cep = 'Este campo está vazio, preenche-o!';
  }
  if (novoTutor.value.endereco.numero.trim() === '') {
    eValido = false;
    erro.value.endereco.numero = 'Este campo está vazio, preenche-o!';
  }
  if (novoTutor.value.endereco.bairro.trim() === '') {
    eValido = false;
    erro.value.endereco.bairro = 'Este campo está vazio, preenche-o!';
  }
  if (novoTutor.value.endereco.cidade.trim() === '') {
    eValido = false;
    erro.value.endereco.cidade = 'Este campo está vazio, preenche-o!';
  }
  if (novoTutor.value.endereco.estado.trim() === '') {
    eValido = false;
    erro.value.endereco.estado = 'Este campo está vazio, preenche-o!';
  }
  if (novoTutor.value.endereco.rua.trim() === '') {
    eValido = false;
    erro.value.endereco.rua = 'Este campo está vazio, preenche-o!';
  }
  return eValido;
};

async function salvarTutor() {
  if (validaForm()) {
    await fetch(`${API_URL}/tutores`, {
      method: 'POST',
      headers: {
        'Content-type': 'application/json',
      },
      body: JSON.stringify(novoTutor.value),
    });
    route.push('/tutores');
  }
}

async function carregaCEP() {
  const cep = novoTutor.value.endereco.cep.trim();
  if (cep.length !== 8) {
    return;
  }
  const resposta = await fetch(`https://viacep.com.br/ws/${cep}/json/`);
  const dados = await resposta.json();
  if (dados.erro) {
    erro.value.endereco.cep = 'CEP não encontrado!';
    return;
  }
  novoTutor.value.endereco.rua = dados.logradouro;
  novoTutor.value.endereco.bairro = dados.bairro;
  novoTutor.value.endereco.cidade = dados.localidade;
  novoTutor.value.endereco.estado = dados.uf;
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
      novalidate
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
            :class="{ 'is-invalid': erro.nome }"
            placeholder="Digite o nome completo"
            v-model="novoTutor.nome"
            required
          />
          <span
            class="invalid-feedback"
            v-show="erro.nome"
          >
            {{ erro.nome }}
          </span>
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
            :class="{ 'is-invalid': erro.cnpj_cpf }"
            placeholder="Digite seu cpf..."
            v-model="novoTutor.cnpj_cpf"
            required
          />
          <span
            class="invalid-feedback"
            v-show="erro.cnpj_cpf"
          >
            {{ erro.cnpj_cpf }}
          </span>
        </div>
        <div class="w-100">
          <label
            for="telefone"
            class="form-label"
          >
            Telefone/Whatsapp:
          </label>
          <input
            type="text"
            id="telefone"
            class="form-control"
            :class="{ 'is-invalid': erro.telefone }"
            v-model="novoTutor.telefone"
            required
          />
          <span
            class="invalid-feedback"
            v-show="erro.telefone"
          >
            {{ erro.telefone }}
          </span>
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
            :class="{ 'is-invalid': erro.email }"
            placeholder="Digite seu email..."
            v-model="novoTutor.email"
            required
          />
          <span
            class="invalid-feedback"
            v-show="erro.email"
          >
            {{ erro.email }}
          </span>
        </div>
        <div class="w-100">
          <label
            for="data"
            class="form-label"
          >
            Data de Nascimento:
          </label>
          <input
            type="date"
            id="data"
            class="form-control"
            :class="{ 'is-invalid': erro.data }"
            v-model="novoTutor.data"
            required
          />
          <span
            class="invalid-feedback"
            v-show="erro.data"
          >
            {{ erro.data }}
          </span>
        </div>
      </section>

      <h4 class="mt-3">Endereço</h4>
      <section class="d-flex border-top border-2 gap-5">
        <div class="w-100">
          <label
            for="cep"
            class="form-label"
          >
            CEP:
          </label>
          <input
            type="text"
            id="cep"
            class="form-control"
            :class="{ 'is-invalid': erro.endereco.cep }"
            placeholder="Digite seu cep aqui"
            v-model="novoTutor.endereco.cep"
            @change="carregaCEP()"
            required
          />
          <span
            class="invalid-feedback"
            v-show="erro.endereco.cep"
          >
            {{ erro.endereco.cep }}
          </span>
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
            :class="{ 'is-invalid': erro.endereco.numero }"
            placeholder="Digite o seu número"
            v-model="novoTutor.endereco.numero"
            required
          />
          <span
            class="invalid-feedback"
            v-show="erro.endereco.numero"
          >
            {{ erro.endereco.numero }}
          </span>
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
            :class="{ 'is-invalid': erro.endereco.bairro }"
            placeholder="Digite seu bairro"
            v-model="novoTutor.endereco.bairro"
            required
          />
          <span
            class="invalid-feedback"
            v-show="erro.endereco.bairro"
          >
            {{ erro.endereco.bairro }}
          </span>
        </div>
        <div class="w-100">
          <label
            for="cidade"
            class="form-label"
          >
            Cidade:
          </label>
          <input
            type="text"
            id="cidade"
            class="form-control"
            :class="{ 'is-invalid': erro.endereco.cidade }"
            placeholder="Digite sua cidade"
            v-model="novoTutor.endereco.cidade"
            required
          />
          <span
            class="invalid-feedback"
            v-show="erro.endereco.cidade"
          >
            {{ erro.endereco.cidade }}
          </span>
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
            :class="{ 'is-invalid': erro.endereco.estado }"
            v-model="novoTutor.endereco.estado"
            required
          >
            <option
              v-for="uf in estados"
              :key="uf.uf"
              :value="uf.value"
            >
              {{ uf.uf }}
            </option>
          </select>
          <span
            class="invalid-feedback"
            v-show="erro.endereco.estado"
          >
            {{ erro.endereco.estado }}
          </span>
        </div>
        <div class="w-100">
          <label
            for="rua"
            class="form-label"
          >
            Rua:
          </label>
          <input
            type="text"
            id="rua"
            class="form-control"
            :class="{ 'is-invalid': erro.endereco.rua }"
            v-model="novoTutor.endereco.rua"
            required
          />
          <span
            class="invalid-feedback"
            v-show="erro.endereco.rua"
          >
            {{ erro.endereco.rua }}
          </span>
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
