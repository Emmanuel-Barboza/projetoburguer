<template>
    <div>
        <Message :msg="msg" v-show="msg" />
        <div>
            <form id="form-ham" @submit="createHam">
                <div class="input-container">
                    <label for="name">Nome do Cliente</label>
                    <input type="text" placeholder="Digite o seu Nome" id="name" name="name" v-model="name">
                </div>
                <div class="input-container">
                    <label for="bread">Escolha o Seu Pão:</label>
                    <select name="bread" id="bread" v-model="bread">
                    <option v-for="bread in paes" :key="bread.id" :value="bread.tipo">{{bread.tipo}}</option>
                    </select>
                </div>
                <div class="input-container">
                    <label for="meat">Escolha a Sua Carne:</label>
                    <select name="meat" id="meat" v-model="meat">
                    <option v-for="meat in carnes" :key="meat.id" :value="meat.tipo">{{meat.tipo}}</option>
                    </select>
                </div>
                <div id="opc-container" class="input-container">
                    <label id="opc-type" for="opcionais">Selecione os opcionais:</label>
                    <div class="checkbox-container" v-for="opcional in opcionaisdata" :key="opcional.id">
                        <input type="checkbox" name="opcionais" v-model="opcionais" :value="opcional.tipo">
                        <span>{{opcional.tipo}}</span>
                    </div>
                </div>
                <div class="input-container">
                    <input type="submit" class="submit-btn" value="Finalizar Pedido">
                </div>
            </form>
        </div>
    </div>
</template>

<script>
import Message from "./Message.vue";
export default {
    name: "FormHam",
    data() {
        return {
            paes: null,
            carnes: null,
            opcionaisdata: null,
            name: null,
            bread: null,
            meat: null,
            opcionais: [],
            msg: null
        };
    },
    methods: {
        async getItems() {
            const req = await fetch("http://localhost:3000/ingredientes");
            const data = await req.json();
            this.paes = data.paes;
            this.carnes = data.carnes;
            this.opcionaisdata = data.opcionais;
        },
        async createHam(e) {
            e.preventDefault();
            const data = {
                name: this.name,
                bread: this.bread,
                meat: this.meat,
                opcionais: Array.from(this.opcionais),
                status: "Solicitado"
            };
            const dataJson = JSON.stringify(data);
            const req = await fetch("http://localhost:3000/burgers", {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: dataJson
            });
            const res = await req.json();
            this.msg = `Pedido Nº ${res.id} Realizado com Sucesso`
            setTimeout(()=> this.msg = "",3000)
            this.name = "",
                this.bread = "",
                this.meat = "",
                this.opcionais = "";
            console.log(res);
        }
    },
    mounted() {
        this.getItems();
    },
    components: { Message }
}
</script>

<style scoped>
#form-ham{
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
    border-left: 4px solid #fcba03;
}
input, select{
    padding: 5px 10px;
    width: 300px;
}
#opc-container{
    flex-direction: row;
    flex-wrap: wrap;
}
#opc-type{
    width: 100%;
}
.checkbox-container{
    display: flex;
    align-items: flex-start;
    width: 50%;
    margin-bottom: 20px;
}
.checkbox-container span, .checkbox-container input{
    width: auto;
}
.checkbox-container span{
    margin-left: 6px;
    font-weight: bold;
}

.submit-btn{
    background-color: #222;
    color: #fcba03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 center;
    cursor: pointer;
    transition: .5s;
}
.submit-btn:hover{
    background-color: transparent;
    color: #222;
}
</style>