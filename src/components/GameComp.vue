<template>
<div class="game">
  <div class="game__wrapper">
    <div class="game__title">
      <h3>
        People: {{quantity}}
      </h3>
    </div>
    <div :class="`game__slots game__slots-${quantity}`">
      <div class="game__slotItem" v-for="(item) of props.quantity" :key="item">
        <GameItem ref="childComponentRef" @wheelStopped="alertWinner" :key="item" :config="config" :user="item"/>
      </div>
    </div>
    <button class="game__start" @click="start">Start</button>
  </div>
</div>
</template>

<script lang="ts" setup>
import {ref} from "vue";
import GameItem from "./GameItem.vue";

const props = defineProps({
  quantity: {
    type: Number,
    required: true,
  }
});

const config = ref(Array<number>())
createConfig(11)
const childComponentRef = ref()

function start(){
  for(let i = 0; i < props.quantity; i++){
    childComponentRef.value[i].start()
  }
}

function alertWinner(user: any){
 console.log(user)
}

function createConfig(number: number){
  for(let i = 0; i < number; i++){
    config.value.push(i);
  }
}
</script>

<style scoped lang="scss">
.game{
  width: 100%;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  @media (max-width: 1000px) {
    grid-template-columns: repeat(8, 1fr);
  }

  &__wrapper{
    grid-column: 2/4;
    display: flex;
    flex-direction: column;
    align-items: center;
    @media (max-width: 1000px) {
      grid-column: 2/7;
    }
  }
  &__title{
    color: white;
  }
  &__slots{
    display: grid;
    width: 100%;
    &-2{
      grid-template-columns: repeat(2, 50%);
      column-gap: 20px;
      text-align: center;
    }
    &-3{
      grid-template-columns: repeat(3, 33%);
      column-gap: 20px;
    }
    &-4{
      grid-template-columns: repeat(4, 25%);
      column-gap: 20px;
    }
  }
  &__slotItem{
    width: 100%;
    min-height: 400px;
    align-items: center;
    display: flex;
    border: 1px solid white;
    border-radius: 5px;
    padding: 10px;
  }
  &__start{
    margin-top: 20px;
  }
}
.v-enter-active,
.v-leave-active {
  transition: opacity 0.4s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}
</style>