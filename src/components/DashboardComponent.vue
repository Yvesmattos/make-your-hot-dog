<script setup>
import { onMounted, ref } from "vue";
import MessageComponent from "./MessageComponent.vue";

const data = ref({
  pedidos: null,
  pedido_id: null,
  status: [],
  msg: null,
});

const getPedidos = async () => {
  const req = await fetch("http://localhost:3002/pedidos");
  const res = await req.json();

  data.value.pedidos = res;
  console.log(data.value)
};

const getStatus = async () => {
  const req = await fetch("http://localhost:3002/status");
  const res = await req.json();

  data.value.status = res;
};

const deletar = async (id) => {
  const req = await fetch(`http://localhost:3002/pedidos/${id}`, {
    method: "DELETE",
  });
  const res = await req.json();

  data.value.msg = `Pedido excluído com sucesso`;

  setTimeout(() => {
    data.value.msg = null;
  }, 3000);

  getPedidos();
};

const uptadePedidos = async (event, id) => {
  const option = event.target.value;
  const dataJson = JSON.stringify({ status: option });

  const req = await fetch(`http://localhost:3002/pedidos/${id}`, {
    method: "PATCH",
    headers: { "Content-Type": "application/json" },
    body: dataJson,
  });
  const res = await req.json();

  data.value.msg = `O pedido ${res.id} foi atualizado para ${res.status}`;

  setTimeout(() => {
    data.value.msg = null;
  }, 3000);

  console.log(res);
};

onMounted(() => {
  getPedidos();
  getStatus();
});
</script>

<template>
  <div id="pedido-table">
    <MessageComponent :msg="data.msg" v-show="data.msg" />

    <div>
      <div id="pedido-table-heading">
        <div class="order-id">#:</div>
        <div>Cliente</div>
        <div>Pão</div>
        <div>Salsicha/Linguica</div>
        <div>Extras</div>
        <div>Acões</div>
      </div>
    </div>
    <div id="pedido-table-rows" v-for="pedido in data.pedidos" :key="pedido.id">
      <div class="pedido-table-row">
        <div class="order-number">{{ pedido.id }}</div>
        <div>{{ pedido.nome }}</div>
        <div>{{ pedido.pao }}</div>
        <div>{{ pedido.embutido }}</div>
        <div>
          <ul>
            <li v-for="(extra, index) in pedido.extras" :key="index">
              {{ extra }}
            </li>
          </ul>
        </div>
        <div>
          <select
            name="status"
            class="status"
            @change="updatePedido($event, pedido.id)"
          >
            <option value="">Selecione</option>
            <option
              v-for="s in data.status"
              :key="s.id"
              :value="s.tipo"
              :selected="pedido.status == s.tipo"
            >
              {{ s.tipo }}
            </option>
          </select>
          <button class="delete-btn" @click="deletar(pedido.id)">
            Cancelar
          </button>
        </div>
      </div>
    </div>
  </div>
</template>


<style scoped>
#pedido-table {
  max-width: 1200px;
  margin: 0 auto;
}
#pedido-table-heading,
#pedido-table-rows,
.pedido-table-row {
  display: flex;
  flex-wrap: wrap;
}
#pedido-table-heading {
  font-weight: bold;
  padding: 12px;
  border-bottom: 3px solid #333;
}
.pedido-table-row {
  width: 100%;
  padding: 12px;
  border-bottom: 1px solid #ccc;
}
#pedido-table-heading div,
.pedido-table-row div {
  width: 19%;
}
#pedido-table-heading .order-id,
.pedido-table-row .order-number {
  width: 5%;
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
  transition: 0.5s;
}

.delete-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>