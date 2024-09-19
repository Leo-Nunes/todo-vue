<script setup>
import { reactive } from 'vue';

  const estado = reactive({
    filtro: 'todas',
    tarefaTemp: '',
    tarefas: [
      {
        titulo: 'Abertos',
        finalizadas: false,
      },
      {
        titulo: 'Pendentes',
        finalizadas: false,
      },
      {
        titulo: 'Operadora',
        finalizadas: true,
      }
    ]
    
  })
  
  const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefas => !tarefas.finalizadas)
  }

  const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefas => tarefas.finalizadas)
  }
  
  const getTarefasFiltradas = () => {
    const {filtro} = estado; 

    switch (filtro) {
      case 'pendentes':
        return getTarefasPendentes(); 
      case 'finalizadas': 
        return getTarefasFinalizadas();
      default:
        return estado.tarefas;
    }
  }

  const cadastraTarefa = () => {
    const tarefaNova = {
      titulo: estado.tarefaTemp,
      finalizadas: false
    }
    estado.tarefas.push(tarefaNova);
    estado.tarefaTemp = '';
  }
  
</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light text-warning rounded-3">
      <h1>Check List de Tarefas</h1>
      <p>
        VocÊ possui {{ getTarefasPendentes().length }} tarefas pendentes
      </p>
    </header>
    <form @submit.prevent="cadastraTarefa">
      <div class="row">
        <div class="col">
          <input :value="estado.tarefaTemp" @change="evento => estado.tarefaTemp = evento.target.value" required type="text" placeholder="Digite o Número do Contrato" class="form-control bg-light">
        </div>
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary">Cadastrar</button>
        </div>
        <div class="col-md-2">
          <select @change="evento => estado.filtro = evento.target.value" class="form-control">
            <option value="todas">Todas tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="tarefas in getTarefasFiltradas()">
        <input @change="evento  => tarefas.finalizadas = evento.target.checked" :checked="tarefas.finalizadas" :id="tarefas.titulo" type="checkbox">
        <label :class="{done: tarefas.finalizadas }" class="ms-3" :for="tarefas.titulo">
          {{ tarefas.titulo }}
        </label>
      </li>
    </ul>
  </div>
</template>

<style scoped>
  .done {
    text-decoration: line-through;
  }
</style>
