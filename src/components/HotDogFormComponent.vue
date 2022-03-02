<script setup>
import { onMounted, ref } from "vue";
import MessageComponent from "./MessageComponent.vue";

const data = ref({
  breads: null,
  embutidos: null,
  extrasdata: [],
  name: null,
  bread: null,
  embutido: null,
  extras: [],
  msg: null,
});

const getIngredients = async () => {
  const req = await fetch("http://localhost:3002/ingredientes");
  const dataReq = await req.json();

  data.value.breads = dataReq.paes;
  data.value.embutidos = dataReq.embutidos;
  data.value.extrasdata = dataReq.opcionais;

  console.log(data.value)
};

const createPedido = async (e) => {
  e.preventDefault();

  const dataReq = {
    nome: data.value.name,
    embutido: data.value.embutido,
    pao: data.value.bread,
    extras: Array.from(data.value.extras),
    status: "Solicitado",
  };

  const dataJson = JSON.stringify(dataReq);

  const req = await fetch("http://localhost:3002/pedidos", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: dataJson,
  });

  const res = await req.json();

  data.value = {};
  data.value.msg = `Pedido Nº ${res.id} realizado com sucesso`;

  setTimeout(() => {
    data.value.msg = null;
  }, 3000);

  getIngredients();
};

onMounted(() => {
  getIngredients();
});
</script>

<template>
  <div>
    <MessageComponent :msg="data.msg" v-show="data.msg" />
    <div>
      <form id="pedido-form" @submit="createPedido">
        <div class="input-container">
          <label for="name">Nome do cliente</label>
          <input
            type="text"
            id="name"
            name="name"
            v-model="data.name"
            placeholder="Digite o seu nome:"
          />
        </div>
        <div class="input-container">
          <label for="bread">Escolha o pão</label>
          <select name="bread" id="bread" v-model="data.bread">
            <option value="">Selecione o seu pão</option>
            <option
              v-for="bread in data.breads"
              :key="bread.id"
              :value="bread.tipo"
            >
              {{ bread.tipo }}
            </option>
          </select>
        </div>
        <div class="input-container">
          <label for="embutido">Escolha entre salsicha ou linguiça</label>
          <select name="embutido" id="embutido" v-model="data.embutido">
            <option value="">Selecione entre salsicha e linguiça</option>
            <option
              v-for="embutido in data.embutidos"
              :key="embutido.id"
              :value="embutido.tipo"
            >
              {{ embutido.tipo }}
            </option>
          </select>
        </div>
        <div id="extras-container" class="input-container">
          <label id="extras-title" for="embutido">Escolha os incrementos</label>
          <div
            class="checkbox-container"
            v-for="extra in data.extrasdata"
            :key="extra.id"
          >
            <input
              type="checkbox"
              name="extras"
              v-model="data.extras"
              :value="extra.tipo"
            />
            <span>{{ extra.tipo }}</span>
          </div>
          <div class="input-container">
            <input type="submit" class="submit-btn" value="Criar meu lanche" />
          </div>
        </div>
      </form>
    </div>
  </div>
</template>


<style scoped>
#pedido-form {
  max-width: 400px;
  margin: 0 auto;
}

.input-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}
label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #fcba03;
}

input,
select {
  padding: 5px 10px;
  width: 300px;
}

#extras-container {
  flex-direction: row;
  flex-wrap: wrap;
}

#extras-title {
  width: 100%;
}

.checkbox-container {
  display: flex;
  align-items: flex-start;
  width: 50%;
  margin-bottom: 20px;
}

.checkbox-container span,
.checkbox-container input {
  width: auto;
}

.checkbox-container span {
  margin-left: 6px;
  font-weight: bold;
}

.submit-btn {
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

.submit-btn:hover {
  background: transparent;
  color: #222;
}
</style>