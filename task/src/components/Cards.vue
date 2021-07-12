<template>
  <div :class="'cards ' + (dark ? 'dark' : '')">
    <div class="cards__wrapper card">
      <div
        v-for="(card, indexCard) in cards"
        @drop="onDrop($event, indexCard)"
        @dragover.prevent
        @dragenter.prevent
        :key="card.titleCard"
        class="cards__card"
      >
        <h2 class="cards__title">
          {{ card.titleCard + ` (${card.cards.length})` }}
        </h2>
        <div
          :class="'cards__card-task ' + (dark ? 'dark' : '')"
          v-for="(item, index) in card.cards"
          @dragstart="onDragStart($event, [indexCard, index])"
          draggable="true"
          :key="item.data"
        >
          <div class="cards__top">
            <h3>Задача №{{ item.id }}</h3>
            <div
              :class="
                'cards__prioritet ' +
                  (item.prioritet == 1
                    ? 'green'
                    : item.prioritet == 2
                    ? 'blue'
                    : 'red')
              "
            >
              {{ item.prioritet }}
            </div>
          </div>
          <p class="cards__desc">
            {{ item.description }}
          </p>
          <div class="cards__dates">
            <p>Дата: {{ item.data }}</p>
            <p>Время: {{ item.time }}</p>
          </div>
          <div class="cards__buttons">
            <button
              :disabled="!indexCard"
              v-on:click="changePriority(item, 1, index)"
              class="cards__btn"
            >
              ←
            </button>
            <button
              v-on:click="openModal({ task: item, index: index })"
              class="cards__btn"
            >
              Edit
            </button>
            <button
              v-on:click="
                indexCard == cards.length - 1
                  ? deleteTask(indexCard, index)
                  : changePriority(item, 2, index)
              "
              class="cards__btn"
            >
              {{ indexCard == cards.length - 1 ? "⮿" : "→" }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Cards",
  props: {
    cards: Array,
    dark: Boolean,
    changePriority: Function,
    deleteTask: Function,
    openModal: Function,
  },
  methods: {
    changePrssiority(task, next) {
      if (next == 2) {
        task.priority;
      }
    },
    onDrop(event, id) {
      console.log(id);
      this.$emit("onDrop", { event, id });
    },
    onDragStart(event, cardsId) {
      this.$emit("onDragStart", { event, cardsId });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
$color-light: #f1f9ff;
.cards {
  &__wrapper {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
    grid-gap: 40px;
    @media (max-width: 780px) {
      grid-template-columns: 1fr;
    }
  }
  &__buttons {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 20px;
  }
  &__top {
    display: flex;
    justify-content: space-between;
  }
  &__prioritet {
    color: white;
    text-align: center;
    border-radius: 100%;
    width: 20px;
    height: 20px;
    line-height: 20px;
    &.green {
      background-color: green;
    }
    &.blue {
      background-color: blue;
    }
    &.red {
      background-color: red;
    }
  }
  &__btn:disabled {
    opacity: 0.4;
    cursor: unset;
  }
  &__btn {
    width: 40px;
    height: 40px;
    font-size: 15px;
    border-radius: 4px;
    border: 2px solid blue;
    color: blue;
    cursor: pointer;
    background-color: $color-light;
  }
  &__card {
    min-height: 500px;
    background-color: $color-light;
    padding: 10px;
    border-radius: 4px;
  }
  &__card-task {
    width: 100%;
    border-radius: 4px;
    border: 2px solid blue;
    background-color: #fff;
    padding: 10px;
    margin-top: 20px;
    &:active {
      opacity: 1;
    }
  }
  &__title {
    text-align: center;
    color: blue;
  }
  &.dark &__card {
    background-color: #0d0762;
  }
  &.dark &__title {
    color: white;
  }
  &.dark &__card-task {
    background-color: #757579;
    color: white;
  }
  &.dark &__btn {
    background-color: #11113a;
    color: white;
  }
}
</style>
