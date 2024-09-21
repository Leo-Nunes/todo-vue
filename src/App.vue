<script setup>
  import { reactive } from 'vue';
  import Cabecalho from './components/Cabecalho.vue';
  import Formulario from './components/Formulario.vue';
  import ListaDeTarefas from './components/ListaDeTarefas.vue';

  const estado = reactive({
    filtro: 'todas',
    tarefaTemp: ' ',
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
    estado.tarefaTemp = ' ';
  }
  
</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length"/>
    <Formulario :trocar-filtro="evento => estado.filtro = evento.target.value  " :tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="evento => evento.tarefaTemp = evento.target.value" :cadastra-tarefa="cadastraTarefa"/>
    <ListaDeTarefas :tarefas="getTarefasFiltradas"/>
  </div>
</template>


