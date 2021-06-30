<template>
  <div class="roll">
    <div class="container">
      <h1>Russian roulette</h1>
      <div class="roll__logo"></div>
      <div class="roll__wrapper">
        <div class="roll__wrapper-btn" @click="chargeItem">Зарядить</div>
        <div class="roll__wrapper-btn">
          <span @click="triggerRoll" v-if="this.thing != 2"
            >Нажать на курок</span
          ><span @click="resetRoll" v-if="this.thing == 2">Начать заново?</span>
        </div>
      </div>
      <div class="roll__action">{{ rollInfo }}</div>
    </div>
  </div>
</template>
<script>
export default {
  name: "RevolverRoll",
  props: {
    msg: String,
  },
  data: function () {
    return {
      stackRoll: [],
      itemRoll: "",
      patron: 1,
      rollInfo: "",
      thing: [],
    };
  },
  methods: {
    chargeItem: function () {
      if (this.stackRoll.length < 6) {
        this.stackRoll.push(this.patron++);
        this.rollInfo = "Зарядил патрон";
      } else {
        this.rollInfo = "Барабан полон";
      }
    },
    
    triggerRoll: function () {
      if (this.stackRoll.length > 0) {
        this.stackRoll.pop(this.patron--);
        this.rollInfo = "Успешный выстрел";
        this.thing = [Math.floor(Math.random() * 6) + 1];
        this.triggerRollText = "Нажал на курок";
        if (this.thing == 2) {
          this.rollInfo = "Убит :( , Начать заново?";
          this.triggerRollText = "Заново";
        } else {
          this.rollInfo = "Хух, повезло! :)";
        }
      } else {
        this.rollInfo = "Барабан пуст";
      }
    },

    resetRoll: function () {
      this.stackRoll = [];
      this.stackRoll.length = 0;
      this.thing = 0;
      this.rollInfo = "Не забудь зарядить";
    },
  },
};
</script>
<style scoped lang="scss">
.roll {
  &__logo {
    width: 100%;
    height: 300px;
    background-repeat: no-repeat;
    background-image: url(/clip-2.jpg);
    background-size: contain;
    background-position: center;
  }

  &__wrapper {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 30px;
    &-btn {
      padding: 20px 40px 20px 60px;
      color: #fff;
      background-color: #000;
      border-radius: 10px;
      cursor: pointer;
      position: relative;
      border: 1px solid #000;
      transition: all .4;
      &:hover {
        color: #000;
        background-color: #fff;
        border: 1px solid #000;
      }
      &::before {
        content: "→";
        position: absolute;
        left: 25px;
        top: 50%;
        transform: translateY(-50%);
      }
      & + & {
        margin-left: 30px;
      }
    }
  }
  &__action {
    font-size: 28px;
    text-align: center;
    color: rgb(151, 53, 53);
  }
}
</style>