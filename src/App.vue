<script setup>
import { reactive } from 'vue';

const estado = reactive({
    atividade: '',
    nota: '',
    aprovado: null,
    atividades: []  
});

// Função para adicionar atividade à lista
const adicionarAtividade = () => {
    if (estado.atividade && estado.nota !== '') {
        const aprovado = estado.nota >= 6 ? 'Sim' : 'Não';  
        estado.atividades.push({
            atividade: estado.atividade,
            nota: estado.nota,
            aprovado
        });
        estado.atividade = '';  
        estado.nota = '';
    }
};


const calcularMedia = () => {
    if (estado.atividades.length === 0) return 'N/A';
    const total = estado.atividades.reduce((acc, item) => acc + parseFloat(item.nota), 0);
    return (total / estado.atividades.length).toFixed(2);
};


const verificarAprovacao = () => {
    if (estado.atividades.length === 0) return 'N/A';
    return estado.atividades.every(item => item.nota >= 6) ? 'Aprovado' : 'Reprovado';
};
</script>

<template>
    <form @submit.prevent="adicionarAtividade" id="form">
        <div class="formulario">
            <h2><i>Calcule a média</i></h2>
            <input v-model="estado.atividade" type="text" id="nome" required placeholder="Nome da Atividade">
            <input v-model="estado.nota" id="number" type="number" required placeholder="Nota do Aluno" min="0" max="10">
            <button type="submit">Adicionar</button>
        </div>
    </form>

    <!-- TABELA-->
    <table cellspacing="0">
        <thead>
            <tr>
                <th>Nome da Atividade</th>
                <th>Nota</th>
                <th>Aprovado</th>
            </tr>
        </thead>
        <tbody>
            <!-- Renderiza cada atividade na tabela -->
            <tr v-for="(item, index) in estado.atividades" :key="index">
                <td>{{ item.atividade }}</td>
                <td>{{ item.nota }}</td>
                <td>{{ item.aprovado }}</td>
            </tr>
        </tbody>
        <tfoot>
            <td><u>Média final:</u></td>
            <td>{{ calcularMedia() }}</td>
            <td>{{ verificarAprovacao() }}</td>
        </tfoot>
    </table>
</template>
