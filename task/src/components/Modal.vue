<template>
  <div class="modal">
    <div class="modal__wrapper">
      <div class="modal__block">
        <button class="close__btn" :onClick="closeModal">⮿</button>
        <form>
          <div><label for="1" class="modal__label">Описание</label></div>
          <input v-model="desc" type="text" id="1" class="modal__input" />
          <div><label for="1" class="modal__label">Приоритет</label></div>
          <div class="modal__select">
            <select v-model="pri" class="modal__selector">
              <option value="1">1</option>
              <option value="2">2</option>
              <option value="3">3</option>
            </select>
          </div>
        </form>
        <Button v-on:click="saveChanges" class="modal__btn" text="Сохранить" />
      </div>
    </div>
    <div class="back"></div>
  </div>
</template>

<script>
import Button from "./Button.vue";
export default {
  components: { Button },
  data() {
    return {
      isActive: false,
      desc: "afas",
      pri: "",
    };
  },
  mounted() {
    this.desc = this.description;
    this.pri = this.prioritet;
    const modal = document.querySelector(".modal");
    const modalBlock = document.querySelector(".modal__block");
    modal.addEventListener("click", (event) => {
      if (!event.path.includes(modalBlock)) {
        this.closeModal();
      }
    });
  },
  name: "Cards",
  props: {
    isModal: Boolean,
    addChanges: Function,
    description: String,
    prioritet: Number,
    closeModal: Function,
  },
  methods: {
    saveChanges() {
      if (this.desc == "" || this.pri == "") {
        return;
      }
      this.addChanges(this.desc, this.pri);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
$color-light: #f1f9ff;
.back {
  background-color: gray;
  position: absolute;
  width: 100%;
  height: 100vh;
  left: 0;
  top: 0;
  opacity: 0.3;
}

.close__btn {
  background-color: red;
  color: white;
  border: none;
  border-radius: 100%;
  width: 30px;
  height: 30px;
  outline: none;
  font-size: 20px;
  margin-bottom: 5px;
}
.modal {
  position: fixed;
  width: 100%;
  height: 100vh;
  overflow-y: hidden;
  left: 0px;
  top: 0px;
  z-index: 1000;
  &__btn {
    margin: 0 auto;
    display: block;
  }
  &__wrapper {
    display: flex;
    height: 100%;
    position: relative;
    z-index: 10;
    flex-direction: column;
    justify-content: center;
    padding: 0px 20px;
  }
  &__block {
    width: 400px;
    @media (max-width: 500px) {
      width: 100%;
    }
    margin: 0 auto;
    border-radius: 4px;
    border: 2px solid blue;
    background-color: #fff;
    padding: 40px;
  }
  &__input {
    width: 100%;
    border-radius: 5px;
    height: 40px;
    border: 1px solid blue;
    margin-bottom: 20px;
  }
  &__select {
    margin-bottom: 40px;
    position: relative;
    border-color: blue;
  }
  &__select::after {
    content: "";
    display: block;
    border-style: solid;
    border-width: 6px 5px 0 5px;
    border-color: #000 transparent transparent transparent;
    pointer-events: none;
    position: absolute;
    top: 50%;
    right: 1rem;
    z-index: 1;
    margin-top: -3px;
  }
  &__selector {
    display: block;
    width: 100%;
    padding: 0.75rem 2.5rem 0.75rem 1rem;
    background: none;
    border: 1px solid #ccc;
    border-color: blue;
    border-radius: 3px;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    font-family: inherit;
    font-size: 1rem;
    color: #444;
  }
}
</style>
