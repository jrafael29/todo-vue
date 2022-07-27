<template>
    <div class="area">
        <div class="header" @keypress.enter="adicionarTarefa">
            <div class="date">
                <span class="day">{{dia}}</span>
                <div class="date-second">
                    <span class="month">{{mes}}</span>
                    <span class="year">{{ano}}</span>
                </div>
            </div>

            <div class="day-text">
                {{diaTexto}}
            </div>
        </div>
        <div v-show="mostrarInput" class="form">
            <input ref="campo" :id="id" type="text" v-model="descricao" @keypress.enter="adicionarTarefa" maxlength="25" autofocus placeholder="Pressione enter para salvar">
        </div>


        <div class="body" >
            <div v-for="tarefa in tarefas">
                <TodoTarefa @atualizar-tarefa="atualizarTarefa" @excluir-tarefa="excluirTarefa" :descricao="tarefa.descricao" :feito="tarefa.feito" :id="tarefa.id" />
            </div>
            

        </div>

    </div>
    <div class="mostrar-input-area">
        <button @click="mostrarCampo">+</button>
    </div>
    
</template>

<script>
import TodoTarefa from './TodoTarefa.vue'
export default { 
    data(){
        return {
            contador: 0,
            tarefas: [],
            descricao: '',
            diaTexto: '',
            dia: '',
            ano: '',
            mes: '',
            mostrarInput: false,
        }
    },
    methods: {
        dataAtual(){
            const date = new Date;
            let dateArray = date.toLocaleDateString('pt-BR', {
                    day: 'numeric', weekday: 'long', month: 'long', year: 'numeric'
            })
            .split(" ")
            .filter( (index) => { return index != "de" } )

            this.diaTexto = dateArray[0].split("")
                                        .filter((i) => {
                                            return i !== ','
                                        })
                                        .join("");
            this.dia = dateArray[1];
            this.mes = dateArray[2].split("")
                                    .splice(0, 3)
                                    .join("")
                                    .toUpperCase();
            this.ano = dateArray[3];

            // console.log();

        },
        adicionarTarefa(){
            if(this.descricao.length > 0){
                this.contador++
                this.tarefas.push({
                    id: this.contador,
                    feito: false,
                    descricao: this.descricao,
                })
                this.descricao = ''
            }

            this.mostrarInput = false
        },
        excluirTarefa(id){
            const tarefa = this.tarefas.findIndex( (tarefa) =>  tarefa.id === id )
            this.tarefas.splice(tarefa, 1)
        },
        atualizarTarefa(id){
            const tarefa = this.tarefas.findIndex( (tarefa) =>  tarefa.id === id )

            this.tarefas[tarefa].feito = !this.tarefas[tarefa].feito

        },
        ordenarTarefas(){
            const t = this.tarefas.sort( (tarefa)=>{
                return tarefa.feito == true
            } )
        },
        mostrarCampo(){
            this.mostrarInput = !this.mostrarInput
        }
    },
    components: {
        TodoTarefa,
    },
    mounted(){
        this.dataAtual();
        this.ordenarTarefas();
    },
    beforeUpdate(){
        this.ordenarTarefas();

    }
}
</script>

<style scoped>
.area{
    min-height: 500px;
    max-height: 500px;
    width: 380px;
    overflow-y:scroll;
    padding: 30px;
    background-color: #FFFFFF;
    color: #5D6070;
}
.area .header{
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.area .header input{
    width: 79%;
    padding: 8px;
    outline: none;
    border: 0;
}
.area .header button{
    width: 20%;
    padding: 8px;
    background-color: goldenrod;
    border: 0;
}
.area .header button:hover{
    background-color: rgba(207, 157, 30, 0.788);
}
.area .body{
    padding: 5px;
    margin-top: 15px;
    max-height: 235px;
    overflow-x: auto;
}

.date{
    display: flex;
    gap: 8px;
}
.day{
    font-size: 52px;
    font-weight: bold;
    letter-spacing: -5px;
}

.date-second{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}

.month{
    flex: 1;
    display: flex;
    align-items: end;
    font-size: 16px;
    font-weight: 500;
}

.year{
    flex: 1;
    font-size: 16px;
    font-weight:300;
}

.mostrar-input-area{
    position: absolute;
    z-index: 10;
    bottom: 100px;
    right: 40%;
    width: 50px;
    height: 50px;
    border-radius: 40px;
    opacity: .7;
}
.mostrar-input-area button{
    width: 100%;
    height: 100%;
    border-radius: 40px;
    border: 0;
    background-color: #58b302;
}
.form{
    margin-top: 30px;
}

.form input{
    border: none;
    outline: none;
    background-color: rgba(73, 73, 73, 0.178);
    width: 100%;
    height: 35px;
    border-radius: 8px;
    text-align: center;
}
</style>
