<template>
  <button class="popup__btn" @click="openPopup">Налоговый вычет</button>

  <Popup
    :is-open="isPopupOpen"
    :show="show"
    :count="count"
    @close="isPopupOpen = false"
    @calcul="popupCalcul"
  >
    <template #actions="{ calcul }">
      <input
        placeholder="Введите данные"
        class="popup__input"
        v-model="value"
      />
      <div class="popup__error" v-if="error">
        Поле обязательно для заполнения, мин 20000
      </div>

      <button class="popup__btn-calcul" @click="calcul">Расчитать</button>
      <p class="popup__subtitle" v-if="counts > 0">
        Итого можете внести в качестве досрочных:
      </p>

      <div class="popup__list" v-for="c in counts" :key="c">
        <input type="checkbox" class="popup__checkbox" data-color="green" />
        <label class="popup__text" for="#"
          >{{ count }} рублей
          <span class="popup__span"
            >&nbsp;в {{ c }}-{{ endings[c - 1] }} год
          </span>
        </label>
        <hr />
      </div>

      <div class="popup__list" v-if="counts >= 0">
        <input type="checkbox" class="popup__checkbox" />
        <label class="popup__text" for="#">
          {{ countlast }} рублей<span class="popup__span">
            &nbsp;в {{ counts + 1 }}-{{ endings[counts] || "ый" }} год
          </span>
        </label>
        <hr v-if="counts >= 0" />
      </div>
    </template>
  </Popup>
</template>

<script>
import Popup from "./components/popup.vue";
export default {
  components: { Popup },
  data() {
    return {
      isPopupOpen: false,
      show: false,
      value: "",
      count: 0,

      endings: ["ый", "ой", "ий", "ый", "ый", "ой", "ой", "ой", "ый"],
      field: null,
      error: false,
    };
  },

  methods: {
    openPopup() {
      this.isPopupOpen = true;
    },

    popupCalcul() {
      // Расчет идет на 260000 руб
      if (this.value >= 20000) {
        this.error = false;
        let x = this.value * 12 * 0.13;
        this.counts = Math.ceil(260000 / x) - 1;
        this.count = Math.ceil(x);

        this.countlast = Math.ceil(260000 - x * this.counts);

        document.querySelector(".popup__input").style.borderColor = "#DFE3E6";
        document.querySelector(".popup__input:hover").style.borderColor =
          "#DFE3E6";
      } else {
        this.error = true;
        document.querySelector(".popup__input").style.borderColor = "red";
        document.querySelector(".popup__input:hover").style.borderColor = "red";
      }
    },
  },
};
</script>

<style>
@font-face {
  font-family: "Lab Grotesque";
  src: local("Lab Grotesque Medium"), local("Lab-Grotesque-Medium"),
    url("./assets/fonts/LabGrotesque-Medium.woff2") format("woff2"),
    url("./assets/fonts/LabGrotesque-Medium.woff") format("woff"),
    url("./assets/fonts/LabGrotesque-Medium.ttf") format("truetype");
  font-weight: 500;
  font-style: normal;
}

@font-face {
  font-family: "Lab Grotesque";
  src: local("Lab Grotesque Regular"), local("Lab-Grotesque-Regular"),
    url("./assets/fonts/LabGrotesque-Regular.woff2") format("woff2"),
    url("./assets/fonts/LabGrotesque-Regular.woff") format("woff"),
    url("./assets/fonts/LabGrotesque-Regular.ttf") format("truetype");
  font-weight: 400;
  font-style: normal;
}

body {
  background: linear-gradient(
      255.35deg,
      #dc3131 0.83%,
      rgba(255, 79, 79, 0) 108.93%
    ),
    #ff5e56;
}

.popup__btn {
  width: 198px;
  height: 56px;
  position: absolute;
  left: 0;
  right: 0;
  margin: 0 auto;
  top: 50%;
  transform: translate(0, -50%);
  border: white 1px solid;
  border-radius: 5px;
  background: transparent;
  font-family: Lab Grotesque;
  font-style: normal;
  font-weight: 500;
  font-size: 16px;
  line-height: 24px;
  color: white;
}

.popup__btn:hover {
  background: #ffffff;
  color: #000000;
}

.popup__btn:disabled {
  background: #bec5cc;
  color: #ffffff;
}

.popup__input {
  border: 1px solid #dfe3e6;
  box-sizing: border-box;
  border-radius: 3px;
  height: 40px;
  width: 488px;
  margin-bottom: 8px;
}

.popup__input:hover {
  border: 1px solid #000000;
  box-sizing: border-box;
  border-radius: 3px;
}
.popup__input:focus {
  border: 1px solid #dfe3e6;
  box-sizing: border-box;
  border-radius: 3px;
}

.popup__input:disabled {
  border: 1px solid #808080;
  box-sizing: border-box;
  border-radius: 3px;
}

.popup__btn-calcul {
  font-family: Lab Grotesque;
  font-style: normal;
  font-weight: 500;
  font-size: 14px;
  line-height: 24px;

  color: #ea0029;
  background: none;
  border: none;
  padding: 0px;
}

.popup__btn-calcul:hover {
  font-family: Lab Grotesque;
  font-style: normal;
  font-weight: 500;
  font-size: 14px;
  line-height: 24px;
  color: #f53a31;
  cursor: url("./Pointer.png"), pointer;
}

.popup__btn-calcul:click {
  font-family: Lab Grotesque;
  font-style: normal;
  font-weight: 500;
  font-size: 14px;
  line-height: 24px;
  color: #ea0029;
}

.popup__checkbox {
  background: #ffffff;
  border: 1px solid #dfe3e6;
  box-sizing: border-box;
  border-radius: 6px;

  z-index: -1;
  opacity: 0;
}

.popup__checkbox:hover {
  border: 1px solid #000000;
  box-sizing: border-box;
  border-radius: 6px;
  cursor: url("./Pointer.png"), pointer;
}

.popup__checkbox:disabled {
  background: #bec5cc;
  border: 1px solid #bec5cc;
  box-sizing: border-box;
  border-radius: 6px;
}

.popup__error {
  font-family: Lab Grotesque;
  font-style: normal;
  font-weight: normal;
  font-size: 10px;
  line-height: 12px;
  color: red;
}

.popup__checkbox {
  position: absolute;

  z-index: 1;
  opacity: 0;
  margin: 5px 2px;
  align-items: center;
  user-select: none;
  vertical-align: middle;
}
.popup__checkbox + label {
  display: inline-flex;
  align-items: center;
  user-select: none;
  vertical-align: middle;
}
.popup__checkbox + label::before {
  content: "";
  display: inline-block;
  width: 1em;
  height: 1em;
  flex-shrink: 0;
  flex-grow: 0;
  border: 1px solid #adb5bd;
  border-radius: 0.25em;
  margin-right: 0.5em;
  background-repeat: no-repeat;
  background-position: center center;
  background-size: 50% 50%;
}
.popup__checkbox:checked + label::before {
  background-image: url("./mdi_done.svg");
  background-size: 16px;
  border: 1px solid red;
}

hr {
  margin: 16px 0px;
  height: 1px;

  background: #dfe3e6;
  border: 0;
}

.popup__text {
  font-family: Lab Grotesque;
  font-style: normal;
  font-weight: normal;
  font-size: 14px;
  line-height: 24px;
  color: #000;
}

.popup__span {
  font-family: Lab Grotesque;
  font-style: normal;
  font-weight: normal;
  font-size: 14px;
  line-height: 24px;

  color: #808080;
}

.popup__subtitle {
  font-family: Lab Grotesque;
  font-style: normal;
  font-weight: 500;
  font-size: 14px;
  line-height: 24px;
  margin-bottom: 8px;
}

@media screen and (max-width: 768px) {
  .popup__input {
    width: 389px;
  }
}

@media screen and (max-width: 500px) {
  .popup__input {
    width: 276px;
  }
}

@media screen and (max-width: 350px) {
  .popup__input {
    width: 276px;
  }
}
</style>
