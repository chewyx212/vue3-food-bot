<script setup lang="ts">
import { ref, onMounted } from "vue";

enum OrderEnum {
  "VIP",
  "Normal",
}

enum StatusEnum {
  "PENDING",
  "PROCESSING",
  "COMPLETED",
}

type OrderType = {
  id: number;
  type: OrderEnum;
  status: StatusEnum;
  time: number;
};

type BotType = {
  id: number;
  orderId?: number;
};

const orders = ref<OrderType[]>([]);
const bots = ref<BotType[]>([]);

const checkOperation = () => {
  const pendingOrder = orders.value.filter(
    (order) => order.status == StatusEnum.PENDING
  );
  const emptyBot = bots.value.filter((bot) => bot.orderId == undefined);
  while (emptyBot.length > 0 && pendingOrder.length > 0) {
    var findVip: number = pendingOrder.findIndex(
      (order) => order.type == OrderEnum.VIP
    );
    if (findVip == -1) {
      findVip = 0;
    }

    pendingOrder[findVip].status = StatusEnum.PROCESSING;
    emptyBot[0].orderId = pendingOrder[findVip].id;
    startOperation(pendingOrder[findVip], emptyBot[0]);

    pendingOrder.shift();
    emptyBot.shift();
  }
};

const startOperation = (order: OrderType, bot: BotType) => {
  const operationInterval = setInterval(() => {
    if (order.time > 0) {
      order.time -= 1;
    } else {
      order.status = StatusEnum.COMPLETED;
      bot.orderId = undefined;
      clearInterval(operationInterval);
      checkOperation();
    }
  },1000);
};

const addBot = () => {
  bots.value.push({
    id: bots.value.length + 1,
  });
  checkOperation();
};

const deleteBot = () => {
  if (bots.value.length > 0) {
    const bot: BotType | undefined = bots.value.pop();
    if (bot && bot.orderId) {
      orders.value.find((order) => order.id == bot.orderId!)!.status =
        StatusEnum.PENDING;
    }
  }
  checkOperation();
};

const addNormalOrder = () => {
  orders.value.push({
    id: orders.value.length + 1,
    type: OrderEnum.Normal,
    status: StatusEnum.PENDING,
    time: 10,
  });
  checkOperation();
};

const addVipOrder = () => {
  orders.value.push({
    id: orders.value.length + 1,
    type: OrderEnum.VIP,
    status: StatusEnum.PENDING,
    time: 5,
  });
  checkOperation();
};
</script>

<template>
  <div class="food">
    <div class="food-top">
      <ul class="food-top-list" v-if="orders.length > 0">
        <li v-for="{ id, type, status, time } in orders" :key="id">
          Order {{ id }} - {{ OrderEnum[type] }} - {{ StatusEnum[status] }} -
          {{ status == StatusEnum.PROCESSING ? time :'' }}
        </li>
      </ul>
    </div>
    <div class="food-bot">
      <div class="food-bot-group">
        <div class="food-bot-group-button" @click="deleteBot">- Bot</div>
        <div class="food-bot-group-button" @click="addBot">+ Bot</div>
      </div>

      <div class="food-bot-group">
        <div class="food-bot-group-button" @click="addNormalOrder">
          Add Normal
        </div>
        <div class="food-bot-group-button" @click="addVipOrder">Add VIP</div>
      </div>
    </div>
  </div>
</template>

<style scoped lang="scss">
.food {
  height: 100vh;
  min-width: 50vh;
  padding: 20px 10px;
  background-color: #f2f2f2;
  display: flex;
  justify-content: space-between;
  flex-direction: column;

  &-top {
    display: flex;
    direction: column;

    &-list {
      color: black;
    }
  }
  &-bot {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    &-group {
      display: flex;
      flex-direction: row;
      justify-content: space-evenly;
      padding: 10px 20px;
      align-items: center;

      &-button {
        cursor: pointer;
        margin: 0 5px;
        padding: 6px 18px;
        color: #f2f2f2;
        font-size: 24px;
        border-radius: 6px;
        background-color: rgb(95, 95, 230);
      }
    }
  }
}
</style>