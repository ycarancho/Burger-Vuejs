<template>
    <div id="burger-table">
        <Message :msg="msg" v-show="msg"/>
        <div>
            <div id="burger-table-heading">
                <div class="order-id">#:</div>
                <div>Cliente:</div>
                <div>Pão:</div>
                <div>Carne:</div>
                <div>Opcionais:</div>
                <div>Ações:</div>
            </div>
            <div id="burger-table-rows">
                <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
                    <div class="order-number">{{ burger.id }}</div>
                    <div>{{ burger.nome }}</div>
                    <div>{{ burger.pao }}</div>
                    <div>{{ burger.carne }}</div>
                    <div>
                        <ul>
                            <li v-for="(opcionais, index) in burger.opcionais" :key="index">{{ opcionais }}</li>
                        </ul>
                    </div>
                    <div>
                        <select name="status" id="status" @change="setStatus($event, burger.id)">
                            <option value="#">Selecione</option>
                            <option :value="estado.tipo" v-for="estado in status" :key="estado.id" :selected="burger.status == estado.tipo ">{{ estado.tipo }}</option>
                        </select>
                        <button class="delete-btn" @click="deletarPedido(burger.id)">Cancelar</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script setup>
import Message from './Message.vue';
import { ref, onMounted } from 'vue'

let burgers = ref('');
let burger_id = ref('');
let status = ref([]);
let statusRef = ref('');
let msg = ref('');

async function getPedidos() {
    const responseData = await fetch("http://localhost:3000/burgers");
    await responseData.json()
        .then((response) => {
            burgers.value = response
        })
}

async function getStatus() {
    const responseData = await fetch("http://localhost:3000/status");
    await responseData.json()
        .then((response) => {
            status.value = response
        })
}

async function deletarPedido(id){
    const responseData = await fetch(`http://localhost:3000/burgers/${id}`,{
        method:'DELETE'
    });

    await responseData.json()
    //msg
    //msg no sistema
    msg.value = `O pedido de Nº ${id} foi deletado com sucesso !`
    //limpar msg
    setTimeout(() => {
        msg.value = ""
    }, '3000');
    //chama pedidos
    await getPedidos()
}

async function setStatus(event,id){

    const option = event.target.value
    const dataJson = JSON.stringify({ status: option})
    const responseData = await fetch(`http://localhost:3000/burgers/${id}`,{
        method:"PATCH",
        headers: {"Content-Type": "application/json"},
        body:dataJson
    });

    const response = await responseData.json()
    msg.value = `O pedido de Nº ${response.id} foi atualizado para ${response.status} !`
    //limpar msg
    setTimeout(() => {
        msg.value = ""
    }, '3000');

}

onMounted(() => {
    getPedidos()
    getStatus()
})
</script>
<style scoped>
#burger-table {
    max-width: 1200px;
    margin: 0 auto;
}

#burger-table-heading,
#burger-table-rows,
.burger-table-row {
    display: flex;
    flex-wrap: wrap;
}

#burger-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
}

#burger-table-heading div,
.burger-table-row div {
    width: 19%;
}

.burger-table-row {
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #CCC;
}

#burger-table-heading .order-id,
.burger-table-row .order-number {
    width: 5%;
}

select {
    padding: 12px 6px;
    margin-right: 12px;
}

.delete-btn {
    background-color: #222;
    color: #FCBA03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
}

.delete-btn:hover {
    color: #222;
    background-color: transparent;
}
</style>