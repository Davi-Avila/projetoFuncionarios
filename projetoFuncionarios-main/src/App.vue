<script setup lang="ts">
import { ref } from 'vue'
import type Funcionario from './models/Funcionario'

const funcionario = ref({} as Funcionario)
const lista = ref([] as Funcionario[])

const ordenacao = ref('C')            //ref da ordem crescente ou decrescente
const campoOrdenacao = ref('nome')    //ref do campo de ordenação

const editIndex = ref<number | null>(null)  //ref para editar funcionário


function salvaFuncionario() {

  if (!funcionario.value.nome) {        //validações
    alert('Nome é obrigatório')
    return
  }

  if (!funcionario.value.cargo) {
    alert('Cargo é obrigatório')
    return
  }

  if (!funcionario.value.email) {
    alert('Email é obrigatório')
    return
  }

  if (!funcionario.value.salario || funcionario.value.salario <= 0) {
    alert('Salário inválido')
    return
  }

  if (editIndex.value !== null) {       
    lista.value[editIndex.value] = { ...funcionario.value }
    editIndex.value = null
  } else {
    lista.value.push(funcionario.value)
    funcionario.value = {} as Funcionario
    ordenaLista()
  }
}


function excluir(nome: string) {
  const index = lista.value.findIndex(f => f.nome === nome)
  if (index > -1) {
    lista.value.splice(index, 1)
  }
}

function editar(item: Funcionario, index: number) {
  funcionario.value = { ...item } 
  editIndex.value = index
}


function ordenaLista() {
  lista.value.sort((a, b) => {
    let resultado = 0

    if (campoOrdenacao.value === 'nome') {
      resultado = a.nome.localeCompare(b.nome)
    }

    if (campoOrdenacao.value === 'cargo') {
      resultado = a.cargo.localeCompare(b.cargo)
    }

    if (campoOrdenacao.value === 'salario') {
      resultado = a.salario - b.salario
    }

    if (ordenacao.value === 'C') {
      return resultado
    } else {
      return -resultado
    }
  })
}
</script>


<template>
  <div class="container">
    <div class="row">
      <div class="col">

        <form @submit.prevent="salvaFuncionario">
          <div class="mb-3">
            <label class="form-label">Nome</label>
            <input v-model="funcionario.nome" type="text" class="form-control">
          </div>

          <div class="mb-3">
            <label class="form-label">Cargo</label>
            <input v-model="funcionario.cargo" type="text" class="form-control">
          </div>

          <div class="mb-3">
            <label class="form-label">Email</label>
            <input v-model="funcionario.email" type="text" class="form-control">
          </div>

          <div class="mb-3">
            <label class="form-label">Salário</label>
            <input v-model="funcionario.salario" type="number" class="form-control">
          </div>

          <button type="submit" class="btn btn-primary">Enviar</button>
        </form>

        <div class="mt-3">
          <label class="form-label">Ordenação</label>
          <select v-model="ordenacao" @change="ordenaLista" class="form-select">
            <option value="C">Crescente</option>
            <option value="D">Decrescente</option>
          </select>
        </div>
        <div class="mt-3">
          <label class="form-label">Campo</label>
          <select v-model="campoOrdenacao" @change="ordenaLista" class="form-select">
            <option value="nome">Nome</option>
            <option value="cargo">Cargo</option>
            <option value="salario">Salário</option>
          </select>
        </div>

        <table class="table mt-3">
          <thead>
            <tr>
              <th>Nome</th>
              <th>Cargo</th>
              <th>Email</th>
              <th>Salário</th>
              <th>Ações</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in lista" :key="item.nome">
              <td>{{ item.nome }}</td>
              <td>{{ item.cargo }}</td>
              <td>{{ item.email }}</td>
              <td>R$ {{ item.salario }}</td>
              <td>
                <button @click="excluir(item.nome)" class="btn btn-danger">
                  Excluir
                </button>
                <button @click="editar(item, index)" class="btn btn-warning me-2">
                  Editar
                </button>

              </td>
            </tr>
          </tbody>
        </table>

      </div>
    </div>
  </div>
</template>
