<script setup lang="ts">
import {ref} from 'vue';
import type Funcionario from './models/Funcionario';


const funcionario = ref({} as Funcionario);
const lista = ref([] as Funcionario[])

function salvaFuncionario() {
  lista.value.push(funcionario.value);
  funcionario.value = {} as Funcionario;
}



</script>


<template>
  <div class="container">
    <div class="row">
      <div class="col">
        <form @submit.prevent="salvaFuncionario">
          <div class="mb-3">
            <label for="nome" class="form-label">Nome</label>
            <input v-model="funcionario.nome" type="text" class="form-control" id="nome">
          </div>
          <div class="mb-3">
            <label for="cargo" class="form-label">Cargo</label>
            <input v-model="funcionario.cargo" type="text" class="form-control" id="cargo">
          </div>
          <div class="mb-3">
            <label for="email" class="form-label">Email</label>
            <input v-model="funcionario.email" type="text" class="form-control" id="email">
          </div>
          <div class="mb-3">
            <label for="salario" class="form-label">Salario</label>
            <input v-model="funcionario.salario" type="number" class="form-control" id="salario">
          </div>
          <button type="submit" class="btn btn-primary">Enviar</button>
        </form>
        <ol>
          <li v-for="item in lista" :key="item.nome">
            {{ item.nome }} - R$ {{ item.cargo }} - {{ item.email }}
          </li>
        </ol>
      </div>
    </div>
  </div>
</template>
