<template>
    <div id="app">
        <h1>Tarefas</h1>
        <TaskProgress :progress="progress" />
        <!-- tratar o evento -->
        <NewTask @taskAdded="addTask" />
        <TaskGrid
            :tasks="tasks"
            @taskDeleted="deleteTask"
            @taskStateChanged="toggleTaskState"
        />
    </div>
</template>

<script>
// import
import TaskProgress from "./components/TaskProgress";
import TaskGrid from "./components/TaskGrid";
import NewTask from "./components/NewTask";

// export
export default {
    // registrar components
    components: {
        TaskGrid,
        NewTask,
        TaskProgress
    },
    data() {
        return {
            // lista de Tarefas
            tasks: []
        };
    },
    watch: {
        // fazer um watch profundo
        tasks: {
            // converter a task pra json e armazenar no localStorage com a chave tasks
            deep: true,
            handler() {
                localStorage.setItem("tasks", JSON.stringify(this.tasks));
            }
        }
    },
    computed: {
        progress() {
            const total = this.tasks.length;
            const done = this.tasks.filter(t => !t.pending).length;
            return Math.round((done / total) * 100) || 0;
        }
    },
    methods: {
        addTask(task) {
            // fazer a verificação se a task já existe
            const sameName = t => t.name === task.name;
            // se nenhum elemento tem o mesmo nome da task que to querendo add
            const reallyNew = this.tasks.filter(sameName).length === 0;

            if (reallyNew) {
                this.tasks.push({
                    name: task.name,
                    pending: task.pending || true //se nao tiver pending sera por padrão true
                });
            }
            // reallyNew && //se o primeiro elemento for verdadeiro
            //     this.tasks.push({
            //         name: task.name,
            //         pending: task.pending || true
            //     });
        },
        deleteTask(id) {
            // quantidade de elementos a serem excluidos
            const quantElements = 1;

            // id do elemento a ser excluido
            this.tasks.splice(id, quantElements);
        },
        // alternar o estado da task
        toggleTaskState(id) {
            this.tasks[id].pending = !this.tasks[id].pending;
        }
    },
    created() {
        const json = localStorage.getItem("tasks");
        this.tasks = JSON.parse(json) || [];
    }
};
</script>

<style>
body {
    font-family: "Lato", sans-serif;
    background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
    color: #fff;
}

#app {
    display: flex;
    flex: 1;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    height: 100vh;
}

#app h1 {
    margin-bottom: 5px;
    font-weight: 300;
    font-size: 3rem;
}
</style>
