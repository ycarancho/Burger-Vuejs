<template>
    <div>
        <p>Componente de mensagem</p>
        <div>
            <form class="burger-form">
                <div class="input-container">
                    <label for="nome">Informe seu nome: </label>
                    <input type="text" id="nome" name="nome" v-model="nome" placeholder="Informe seu Nome">
                </div>
                <div class="input-container">
                    <label for="pao">Escolha o pão: </label>
                    <select name="pao" id="pao" v-model="pao">
                        <option value="">Selecione seu pão:</option>
                        <option v-for="pao in paes" :key="pao.id" :value="pao.tipo">{{pao.tipo}}</option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="carne">Escolha a carne: </label>
                    <select name="carne" id="carne" v-model="carne">
                        <option value="">Selecione a carne:</option>
                        <option v-for="carne in carnes" :key="carne.id" :value="carne.tipo">{{carne.tipo}}</option>
                    </select>
                </div>
                <div id="opcionais-container" class="input-container">
                    <label id="opcionais-titel" for="opcionais">Selecione os opcionais: </label>
                    <div class="checkbox-container" v-for="opcional in opcionaisData" :key="opcional.id">
                        <input type="checkbox" name="opcionais" id="opcionais" v-model="opcionais" :value="opcional.tipo">
                        <span>{{opcional.tipo}}</span>
                    </div>
                </div>
                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Criar o Burguer" >
                </div>
            </form>
        </div>
    </div>

</template>

<script setup>
import { ref, onMounted } from 'vue'

let pao = ref('');
let carne = ref('');
let opcionais = ref([])
const opcionaisData = ref([])
const paes = ref('')
const carnes = ref('')


async function getIngredients(){
    const responseData = await fetch("http://localhost:3000/ingredientes");
    await responseData.json()
    .then((response) => {
        paes.value = response.paes
        carnes.value = response.carnes
        opcionaisData.value = response.opcionais
    })

}

onMounted(async() => {
    await getIngredients()
})

</script>

<style scoped>
.burger-form{
    max-width: 400px;
    margin: 0 auto;
}

.input-container{
    display: flex;
    flex-direction: column;
    margin-bottom: 20px;
}

label{
    font-weight: bold;
    margin-bottom: 15px;
    color: #222;
    padding: 5px 10px;
    border-left: 4px solid #FCBA03;
    
}

input, select{
    padding: 5px 10px;
    width: 350px;
    margin: 0 auto;
}

#opcionais-container{
flex-direction: row;
flex-wrap: wrap;
}

.checkbox-container{
    align-items: flex-start;
    width: 50%;
    margin-bottom: 20px;
}

.checkbox-container span,
.checkbox-container input{
    width: auto;
}

.checkbox-container span{
    margin-left: 6px;
    font-weight: bold;
}

.submit-btn{
    background-color: #222;
    color: #FCAB03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
}

.submit-btn:hover{
    background-color: transparent;
    color: #222;
}
</style>