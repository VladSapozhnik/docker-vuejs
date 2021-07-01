<template>
  <div class="popup" >
    <div class="container">
        <div class="menu" v-if="!pending">
            <ul>
                <div v-if="hrefItem == ''" class="menu-title">Главная</div>
                <li v-for="menu in menuData.list" :key="menu.id"> 
                    <a v-if="hrefItem == ''" @click="nextStep(menu.href)">{{menu.title}}</a>
                    <a v-if="hrefItem != '' && menu.href == hrefItem" @click="hrefItem = ''" class="step-back">Назад</a>
                    <ul>
                        <li v-for="item in menu.item" :key="item.id">
                            <a v-if="menu.href == hrefItem" @click="nextStep(item.href)">{{item.title}}</a>
                            <a v-if="hrefItem != '' && item.href == hrefItem" @click="backStep(menu.href)" class="step-back">{{menu.stepBack}}</a>
                            <ul v-if="item.href == hrefItem">
                                <li v-for="result in item.result" :key="result.id">
                                    <a>{{result.title}}</a>
                                </li>
                            </ul>
                        </li>
                    </ul>
                </li>
            </ul>
        </div>
        <slot></slot>
        <div class="popup__head">Услуги</div>    
        <div class="popup__head">Новости</div>
        <div class="popup__head">Обучение</div>
        <div class="popup__head">Соц.проект</div>
        <div class="popup__head">О нас</div>
        <div class="popup__head">Нормативная база</div>
        <div class="popup__head">Вакансии</div>
        <div class="popup__head">Контакты</div>
    </div>
  </div>
</template>
<script>

export default {
  name: "Popup",
  props: {
    msg: String,
  },
  data: function () {
    return {
        pending: true,
        menuData: {},
        hrefItem: ''
    };
  },
  created: function () {
    this.axios.get("./static/menu.json").then((response) => {
        this.menuData = response.data;
        this.pending = false;
    });
  },
  methods: {
    nextStep: function (next) {
        this.hrefItem = next
    },
    backStep: function (back) {
        this.hrefItem = back 
    }
  }
};
</script>
<style scoped lang="scss">
.popup {
    &__head {
        padding: 16px 16px 15px;
        text-align: left;
        cursor: pointer;
        font-weight: 500;
        font-size: 14px;
        line-height: 19px;
        color: #323232;
    }
    ul {
        list-style-type: none;
        padding: 0;
        margin: 0;
    }
    li {
        padding: 0;
        margin: 0;
        text-align: left;
        font-size: 15px;
        cursor: pointer;
    }
    a {
        color: #000;
        text-decoration: none;
        text-align: left;
        padding: 10px 0;
        display: flex;
        align-items: center;
    }

    .menu-title {
        padding: 5px 10px;
        border: 1px solid green;
        display: block;
        width: 140px;
        margin-bottom: 5px;
    }

    a.step-back {
        border: 1px solid green;
        padding: 5px 10px;
        display: flex;
        justify-content: center;
        align-items: center;
        width: 140px;
        margin-bottom: 5px;
        &:hover {
            background-color: green;
            color: #fff;
        }
    }

    .menu {
        width: 50%;
        li a {
            border-bottom: 1px solid green;
            padding-left: 10px;
        }
    }
}
</style>