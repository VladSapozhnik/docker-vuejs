<template>
  <div class="stack">
    <div class="container">
      <h1>Stack - обойма</h1>
      <div class="stack__logo"></div>
      <div class="stack__patron" >Патронов заряжено: {{patron}}</div>
      <div class="stack__wrapper">
        <div class="stack__wrapper-btn" @click="pushItem">Зарядить</div>
        <div class="stack__wrapper-btn" @click="popItem">Нажать на курок</div>
        <div class="stack__wrapper-btn" @click="infoItem">Проверить магазин</div>
      </div>
      <div class="stack__action">{{statusPatron}}{{action}}</div>
      <div class="stack__check">{{check}}</div>
      <div class="toTop" @click="toTop">на верх</div>
    </div>
  </div>
</template>
<script>
export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  data: function () {
    return {
      stack: [],
      item: '',
      patron: 0,
      action: '',
      check: '',
      statusPatron: ''
    }
  },
  methods: {
    pushItem:function () {
        if (this.stack.length < 30) {
          this.stack.push(this.item + this.patron);
          this.patron++;
          this.action = 'Зарядил';
        } 
    },

    popItem:function () {
      if (this.stack.length > 0) { 
          this.stack.pop();
          this.patron--;
          this.action = ' выстрел! ';
          if (this.patron == 2) {
            this.statusPatron = 'Холостой'
          } else if (this.patron == 5) {
            this.statusPatron = 'Холостой'
          } else if (this.patron == 8) {
            this.statusPatron = 'Холостой'
          } else if (this.patron == 12) {
            this.statusPatron = 'Холостой'
          } else if (this.patron == 18) {
            this.statusPatron = 'Холостой'
          } else if (this.patron == 24) {
            this.statusPatron = 'Холостой'
          } else if (this.patron == 27) {
            this.statusPatron = 'Холостой'
          } else {
            this.statusPatron = 'Успешный'
          }
      } else {
        this.patron == 0; 
        this.action = 'Выстрел невозможен, магазин пуст!'; 
        this.statusPatron = '';
      }
    },

    infoItem:function () {
      if (this.stack.length == 0) {
        this.check = 'Пуст.'
      } else if (this.stack.length < 10) {
        this.check = 'Одна четвертая.'
      } else if (this.stack.length < 18) {
        this.check = 'Пол магазина.'
      } else if (this.stack.length < 25) {
        this.check = 'Три четвертых.'
      } else if (this.stack.length < 29) {
        this.check = 'Почти полный.'
      } else {
        this.check = 'Полный.'
      }
    },

    toTop:function () {
      window.scrollTo({
        top: -500000,
        left: 0,
        behavior: "smooth"
      })
    }
  },
};
</script>
<style scoped lang="scss">
.container {
  max-width: 920px;
  margin: 0 auto;
  border: 1px solid #000;
  border-top: none;
  padding: 50px 0;
}
.stack {
  min-height: 800px;
  &__logo {
    width: 100%;
    height: 300px;
    background-repeat: no-repeat;
    background-image: url(/clip.jpg);
    background-size: contain;
    background-position: center;
  }
  &__patron {
    font-size: 28px;
    margin-bottom: 30px;
  }
  &__wrapper {
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 30px;
    &-btn {
      padding: 20px 40px;
      color: #fff;
      background-color: #000;
      border-radius: 10px;
      cursor: pointer;
      & + & {
        margin-left: 30px;
      }
    }
  } 
  &__check {
    font-size: 28px;
    text-align: center;
    margin-bottom: 30px;
    color: rgb(46, 161, 98);
  }
  &__action {
    font-size: 28px;
    text-align: center;
    color: rgb(151, 53, 53);
  }

  .toTop {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100px;
    height: 50px;
    color: #fff;
    margin: 0 auto;
    border-radius: 10px;
    background-color: rgb(32, 167, 32);
    position: relative;
    padding-left: 15px;
    &::before {
      content: '▲';
      position: absolute;
      left: 13px;
      top: 50%;
      transform: translateY(-50%);
    }
  }
}
</style>
