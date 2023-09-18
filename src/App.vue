<script setup>
import { reactive } from 'vue';
    //criando tarefas
    const estado = reactive({
        filtro: 'todas',
        tarefaTemp: '',
        tarefas: [
            {
                titulo: 'Estudar ES6',
                finalizada: false,
            },
            {
                titulo: 'Estudar SASS',
                finalizada: false,
            },
            {
                titulo: 'Ir para a academia',
                finalizada: true,
            }
        ]
    })

    const getTarefasPendentes =() => {
        return estado.tarefas.filter(tarefa => !tarefa.finalizada)
    }

    const getTarefasFinalizadas = () => {
        return estado.tarefas.filter(tarefa=> tarefa.finalizada)
    }

    //aqui procuramos onde finalizadas foi igual a true

    const getTarefasFiltradas = () => {
        const {filtro} = estado;

        switch (filtro){
            case 'pendentes':
                return getTarefasPendentes();
            case 'finalizadas':
                return getTarefasFinalizadas();
            default: 
            return estado.tarefas
        }
    }
    //adicionando tarefas. Usei o prevent no formulário para não recarregar a página ao clicar submit
    //reatividade
    const cadastraTarefa = () => {
        
        const tarefaNova = {
            titulo: estado.tarefaTemp,
            finalizada: false,
        }
        estado.tarefas.push(tarefaNova);
        estado.tarefaTemp = '';
    }

</script>

<template>  
    <!-- aqui mostra quantas tarefas possuem -->
    <div class="container">
        <header class="p-5 mb-4 mt-4 bg-light rounded-3">
            <h1>Minhas tarefas</h1>
            <p>
                Você possui {{ getTarefasPendentes().length }} tarefas pendentes
            </p>
        </header>
    </div>
    <!-- criando formulário para envio de tarefas e um checkbox para verificar as tarefas pendentes, finalizadas e etc. -->
    <form @submit.prevent="cadastraTarefa">
        <div class="row">
            <div class="col">
                <input :value="estado.tarefaTemp" @change="evento => estado.tarefaTemp= evento.target.value" required type="text" placeholder="Digite aqui a descrição da tarefa" class="form-control">
            </div>
            <div class="col-md-2">
                <button type="submit" class="btn btn-primary">Cadastrar</button>
            </div>
            <div class="col-md-2">
                <select @change="evento => estado.filtro = evento.target.value" class="form-control">
                    <option value="todas">Todas tarefas</option>
                    <option value="pendentes">Todas pendentes</option>
                    <option value="finalizadas">Todas finalizadas</option>
                </select>
            </div>
        </div>
    </form>
    
    <ul class="list-ground mt-4">
        <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()">
            <input @change="evento => tarefa.finalizada = evento.target.checked" :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox">
            <label :class="{done: tarefa.finalizada }" class="ms-3" :for="tarefa.titulo">
                {{ tarefa.titulo }}
            </label>
        </li>
    </ul>
</template>

<style scoped>
    .done {
        text-decoration: line-through;
    }
</style>
