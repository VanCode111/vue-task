<template>
  <div class="home">
    <div class="container">
      <Modal
        :closeModal="closeModal"
        :description="description"
        :prioritet="prioritet"
        :addChanges="addChanges"
        :task="task"
        v-if="isModal"
      />
      <Button class="home__btn" :oClick="openModal" text="Создать задачу" />
      <Cards
        :dark="dark"
        @onDragStart="onDragStart($event)"
        @onDrop="onDrop($event)"
        :deleteTask="deleteTask"
        :openModal="openModal"
        :changePriority="changePriority"
        :cards="cards"
      />
    </div>
  </div>
</template>

<script>
import Cards from "../components/Cards";
import Button from "../components/Button";
import Modal from "../components/Modal";

export default {
  name: "Home",
  data() {
    return {
      description: "",
      priority: "",
      prioritet: 1,
      edit: false,
      isModal: false,
      index: 0,
      length: 0,
      task: undefined,
      cards: [
        {
          titleCard: "План",
          cards: [],
        },
        {
          titleCard: "В работе",
          cards: [],
        },
        {
          titleCard: "Готово",
          cards: [],
        },
      ],
    };
  },
  components: {
    Cards,
    Button,
    Modal,
  },
  mounted() {
    this.length = localStorage.getItem("length") || 0;
    if (localStorage.getItem("cards")) {
      this.cards = JSON.parse(localStorage.getItem("cards"));
    }
  },
  props: ["dark"],
  methods: {
    onDragStart({ event, cardsId }) {
      event.dataTransfer.setData("IdTask", cardsId.join(""));
    },
    onDrop({ event, id }) {
      let cardsId = event.dataTransfer.getData("IdTask").split("");
      const card = this.cards[cardsId[0]].cards[cardsId[1]];
      this.cards[cardsId[0]].cards.splice(cardsId[1], 1);
      card.priority = id + 1;
      this.cards[id].cards.push(card);
    },
    addChanges(description, prioritet) {
      console.log("изменился");
      if (this.edit) {
        this.task.description = description;
        this.edit = false;
        this.task.prioritet = prioritet;
        console.log(prioritet);
      } else {
        this.length++;
        localStorage.setItem("length", this.length);
        const date = new Date();
        const time = `${date.getHours()}:${date.getMinutes()}`;
        const dataCalendar = date
          .toISOString()
          .replace("-", ".")
          .split("T")[0]
          .replace("-", ".")
          .split(".")
          .reverse()
          .join(".");
        let task = {
          description,
          data: dataCalendar,
          priority: 1,
          id: this.length,
          time: time,
          prioritet: prioritet,
        };
        this.cards[0].cards.push(task);
      }
      localStorage.setItem("cards", JSON.stringify(this.cards));
      this.closeModal();
    },
    changePriority(task, next, index) {
      this.cards[task.priority - 1].cards.splice(index, 1);
      if (next == 2) {
        this.cards[task.priority].cards.push(task);
        task.priority++;
      } else {
        this.cards[task.priority - 2].cards.push(task);
        task.priority--;
      }
      localStorage.setItem("cards", JSON.stringify(this.cards));
    },
    openModal({ task, index }) {
      if (task) {
        this.index = index;
        this.task = task;
        this.description = task.description;
        this.prioritet = task.prioritet;
        this.edit = true;
      }
      this.isModal = true;
    },
    deleteTask(indexCard, index) {
      this.cards[indexCard].cards.splice(index, 1);
      localStorage.setItem("cards", JSON.stringify(this.cards));
    },
    closeModal() {
      this.isModal = false;
      this.description = "";
      this.priority = "";
      this.prioritet = 1;
      document.removeEventListener("click", this.eventModal);
      this.edit = false;
    },
  },
};
</script>

<style scoped lang="scss">
$color-blue: #1d73bc;

[draggable="true"] {
  user-select: none;
}
.container {
  max-width: 1000px;
  margin: 0 auto;
  padding: 0px 20px;
}
.home {
  padding-top: 40px;
  padding-bottom: 20px;
  &__btn {
    background-color: $color-blue;
    padding: 10px 20px;
    color: white;
    border: none;
    cursor: pointer;
    border-radius: 4px;
    margin-bottom: 20px;
  }
}
</style>
