<template>
    <div class="tarefa">
        <div v-if=feito>
            <label :for="id"><div class="feito"></div></label>
            <input @click="$emit('atualizarTarefa', id)" type="checkbox" name="" :id="id" checked>
        </div>
        <div v-else>
            <label :for="id"><div class="nao-feito"></div></label>
            <input @click="$emit('atualizarTarefa', id)" type="checkbox" name="" :id="id" >
        </div>

        <div>
            <span class="descricao">
            <p :class="{'feito': feito}">{{descricao}}</p>
        </span>
        </div>
        
        
        
        <button @click="$emit('excluirTarefa', id)">x</button>
    </div>
</template>

<script>

export default {
    props: {
        descricao: String,
        feito: Boolean,
        id: Number,
    },
    data(){
        return {

        }
    }
}
</script>

<style scoped>
.tarefa{
    padding: 10px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    color: #5D6070;

    overflow-wrap: break-word;
    word-wrap: break-word;
}
.tarefa button{
    width: 20px;
    background: transparent;
    border: 0;
    border-radius: 5px;
    color: black;
    display: flex;
    justify-content: center;
    align-items: center;
    opacity: .2;
}
.descricao p{
    font-size: 18px;
    word-wrap: break-word;
    word-break:break-all;
    text-align: center;
    padding: 0 15px;
}
.tarefa input[type=checkbox]{
    background-color: yellow;
    padding: 8px;
    display: none;
}

label .feito {
    width: 25px;
    height: 25px;
    border: 1px solid #111;
    border-radius: 20px;
}

label .nao-feito {
    width: 25px;
    height: 25px;
    border: 1px solid rgb(95, 187, 10);
    border-radius: 20px;
    
}

.feito{
    opacity: .2;
}
</style>