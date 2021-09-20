<template>
  <div v-if="isOpen" class="backdrop" @click="close">
    <div class="popup" @click.stop>
      <button
        type="button"
        class="popup__btn-close"
        @click="close"
        aria-label="Close modal"
      >
        <img src="../Vector.jpg" alt="123" />
      </button>
      <p class="popup__heading">Налоговый вычет</p>
      <p class="popup__text1">
        Используйте налоговый вычет чтобы погасить ипотеку досрочно. Размер
        налогового вычета составляет не более 13% от своего официального
        годового дохода.
      </p>

      <p class="popup__subtitle">Ваша зарплата в месяц</p>
      <slot name="actions" :calcul="calcul"> </slot>
      <div class="popup__wrapper">
        <p class="popup__subtitle">Что уменьшаем</p>
        <button class="popup__btn-tags pay" @click="pay">Платеж</button>
        <button class="popup__btn-tags term" @click="term">Срок</button>
      </div>
      <button class="popup__btn-add" @click="add">Добавить</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    isOpen: {
      type: Boolean,
      required: true,
    },
    show: {
      type: Boolean,
      required: true,
    },
    count: {
      type: Number,
    },
  },

  emits: {
    close: null,
    calcul: null,
    add: null,
    term: null,
    payment: null,
  },
  mounted() {
    document.addEventListener("keydown", this.handleKeydown);
  },
  beforeUnmount() {
    document.removeEventListener("keydown", this.handleKeydown);
  },

  methods: {
    handleKeydown(e) {
      if (this.isOpen && e.key === "Escape") {
        this.close();
      }
    },

    close() {
      this.$emit("close");
    },

    calcul() {
      this.$emit("calcul");
    },
    add() {
      this.$emit("add");
    },
    term() {
      this.$emit("term");
    },
    openModal() {
      this.$emit("input", "test");
    },
  },
};
</script>

<style>
.popup {
  width: 552px;
  overflow: auto;
  max-height: 100%;
  position: absolute;
  left: 0;
  right: 0;
  margin: 0 auto;
  top: 50%;
  transform: translate(0, -50%);

  background-color: white;
  padding: 32px;
  border-radius: 10px;
  box-sizing: border-box;
}
.popup__btn-close {
  position: absolute;
  width: 18px;
  height: 18px;
  top: 20px;
  right: 30px;
  background: none;
  border: none;
}

.popup__heading {
  font-family: Lab Grotesque;
  font-style: normal;
  font-weight: 500;
  font-size: 28px;
  line-height: 40px;
  margin: 0;
}

.popup__subtitle {
  font-family: Lab Grotesque;
  font-style: normal;
  font-weight: 500;
  font-size: 14px;
  line-height: 24px;
  margin-bottom: 8px;
}

.popup__text1 {
  margin: 0;
  width: 488px;
  height: 72px;
  left: 32px;
  top: calc(50% - 72px / 2 - 114px);
  font-family: Lab Grotesque;
  font-style: normal;
  font-weight: normal;
  font-size: 14px;
  line-height: 24px;

  color: #808080;
}

.backdrop {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.8);
  z-index: 100;
}

.popup__wrapper {
  display: flex;
  align-items: center;
}

.popup__btn-tags {
  font-family: Lab Grotesque;
  font-style: normal;
  font-weight: normal;
  font-size: 14px;
  line-height: 24px;

  text-align: center;
  border: none;
  color: #ffffff;
  display: flex;
  flex-direction: row;
  align-items: center;
  height: 36px;
  margin: 0px 16px 0px 16px;
  background: linear-gradient(
      255.35deg,
      #dc3131 0.83%,
      rgba(255, 79, 79, 0) 108.93%
    ),
    #ff5e56;
  border-radius: 50px;
}

.popup__btn-tags:hover {
  background: #dfe3e6;
  color: #000000;
  cursor: url("../Pointer.png"), pointer;
}
.popup__btn-tags:active {
  background: linear-gradient(
      255.35deg,
      #dc3131 0.83%,
      rgba(255, 79, 79, 0) 108.93%
    ),
    #ff5e56;
  color: #ffffff;
}
.popup__btn-tags:disabled {
  background: #eef0f2;
  color: #000000;
}

.popup__btn-add {
  font-family: Lab Grotesque;
  font-style: normal;
  font-weight: 500;
  font-size: 16px;
  line-height: 24px;
  background: linear-gradient(
      255.35deg,
      #dc3131 0.83%,
      rgba(255, 79, 79, 0) 108.93%
    ),
    #ff5e56;
  box-shadow: 0px 0px 24px rgba(234, 0, 41, 0.33);
  border-radius: 6px;
  color: #ffffff;
  border: none;
  width: 488px;
  height: 56px;
  margin-top: 40px;
}
.popup__btn-add:hover {
  background: #ea0029;
  box-shadow: 0px 0px 24px rgba(234, 0, 41, 0.33);
  border-radius: 6px;
  cursor: url("../Pointer.png"), pointer;
}
.popup__btn-add:active {
  background: #ea0029;
  box-shadow: 0px 0px 24px rgba(234, 0, 41, 0.33);
  border-radius: 6px;
}
.popup__btn-add:disabled {
  background: #bec5cc;
}

@media screen and (max-width: 768px) {
  .popup {
    width: 453px;
  }

  .popup__text1 {
    width: 389px;
  }

  .popup__btn-add {
    width: 389px;
  }
}

@media screen and (max-width: 500px) {
  .popup {
    width: 340px;
  }

  .popup__text1 {
    width: 276px;
    height: auto;
  }

  .popup__btn-add {
    width: 276px;
  }
}

@media screen and (max-width: 350px) {
  .popup {
    width: 100%;

    height: 100%;
    border-radius: 0;
  }

  .popup__text1 {
    width: 276px;
    height: auto;
  }

  .popup__btn-add {
    width: 276px;
  }
}
</style>
