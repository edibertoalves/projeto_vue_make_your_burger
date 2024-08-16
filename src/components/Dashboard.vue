<template>
    <div id="burger-table">
        <Message :msg="msg" v-show="msg" />
        <div>
            <div id="burger-table-heading">
                <div class="order-id">#:</div>
                <div>Cliente:</div>
                <div>Paes:</div>
                <div>Carnes:</div>
                <div>Opicionais:</div>
                <div>Ações:</div>
            </div>
        </div>
        <div id="burger-table-rows">
            <div class="burger-table-row" v-for="burger in burgers" :key="burger.id">
                <div class="order-number">{{ burger.id }}</div>
                <div>{{ burger.nome }}</div>
                <div>{{ burger.pao }}</div>
                <div>{{ burger.carne }}</div>
                <div>
                    <ul>
                        <li v-for="(opcional, index) in burger.opcionais" :key="index">{{ opcional }}</li>
                    </ul>
                </div>
                <div>
                    <select name="" class="status" @change="updateBurger($event, burger.id)">
                        <option v-for="s in status" :key="s.id" :value="s.tipo" :selected="burger.status == s.tipo">{{
                            s.tipo }}</option>
                    </select>
                    <button class="delete-btn" @click="deleteBurger(burger.id)">Cancelar</button>
                </div>
            </div>
        </div>
    </div>

</template>

<script>
import Message from './Message.vue';

export default {
    name: 'Dashboard',
    components: {
        Message
    },
    data() {
        return {
            burgers: null,
            burger_id: null,
            status: [],
            msg: null
        }
    },
    methods: {
        async getPedidos() {
            const req = await fetch('http://localhost:3000/burgers');
            const data = await req.json();

            this.burgers = data;

            console.log("retorno de getPedidos:");
            console.log(this.burgers);
        },

        async getStatus() {
            const req = await fetch('http://localhost:3000/status');
            const data = await req.json();

            this.status = data;

            console.log("retorno de getStatus:");
            console.log(data);
        },

        async deleteBurger(id) {
            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: "DELETE"
            });

            const res = await req.json();

            // colocar uma mensagem de sistema
            this.msg = `O pedido ${id} foi removido com sucesso!`;

            // limpar msg
            setTimeout(() => this.msg = '', 3000);

            // Atualizar a lista de pedidos
            this.getPedidos();

        },
        async updateBurger(event, id) {
            event.preventDefault();

            const option = event.target.value;
            const dataJson = JSON.stringify({ 'status': option });

            const req = await fetch(`http://localhost:3000/burgers/${id}`, {
                method: "PATCH", // como um update, mas só atualiza o campo que tem que ser alterado
                headers: { "Content-Type": "application/json" },
                body: dataJson
            });

            const res = await req.json();

            // colocar uma mensagem de sistema
            this.msg = `O pedido ${id} foi atualizado para ${option}!`;

            // limpar msg
            setTimeout(() => this.msg = '', 3000);

            console.log(res);
        },


    },
    mounted() {
        this.getPedidos();
        this.getStatus();

        // colocar uma mensagem de sistema
        console.log("Dashboard montado");

    }
}

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

#burger-table-heading .order-id,
.burger-table-row .order-number {
    width: 5%;
}

.burger-table-row {
    width: 100%;
    padding: 12px;
    border-bottom: 1px solid #CCC;
}

select {
    padding: 12px 6px;
    margin-right: 12px;
}

.delete-btn {
    background-color: #222;
    color: #fcba03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: .5s;
}

.delete-btn:hover {
    background-color: transparent;
    color: #222;
}
</style>
