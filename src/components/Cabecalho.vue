<script setup>
    const props = defineProps(['tarefasPendentes']);
</script>

<template>
    <header class="p-5 mb-4 mt-4 bg-light text-warning rounded-4">
        <h1>Check List de Tarefas</h1>
        <p>
            VocÊ possui {{ props.tarefasPendentes }} tarefas pendentes
        </p>
    </header>
</template> 