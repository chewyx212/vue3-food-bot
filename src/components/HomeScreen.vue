<template>
  <div class="food">
    <div class="food-top">
      <ul class="food-top-list">
        <li>
          Order 1
        </li>
        <li>
          Order 2
        </li>
        <li>
          Order 3
        </li>
        <li>
          Order 4
        </li>
      </ul>
    </div>
    <div class="food-bot">
      <div class="food-bot-button">Add Normal</div>
      <div class="food-bot-button">Add VIP</div>
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
    flex-direction: row;
    justify-content: center;
    padding: 0px 20px;
    align-items: center;

    &-button {
      margin: 0 5px;
      padding: 6px 18px;
      color: #f2f2f2;
      font-size: 16px;
      border-radius: 6px;
      background-color: rgb(95, 95, 230);
    }
  }
}
</style>
