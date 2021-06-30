<template>
  <div class="popup" >
    <div class="container">
        <div v-if="!pending">
           <ul>
               <li v-for="menu in menuData.list" :key="menu.id">
                   <a :href="menu.href">{{menu.title}}</a>
                   <ul>
                       <li v-for="item in menu.item" :key="item.id">
                           <a :href="item.href">
                               {{item.title}}
                            </a>
                            <ul>
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
        popupHead: '',
        menuData: {}
    };
  },
  created: function () {
    this.axios.get("./static/menu.json").then((response) => {
        this.menuData = response.data;
        this.pending = false;
        console.log(this.menuData);
    });
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
        padding: 0 0 16px 16px;
        margin: 0;
    }
    li {
        padding: 0 0 15px;
        text-align: left;
        font-size: 15px;
    }
    a {
        color: #000;
        text-decoration: none;
        text-align: left;
        padding: 0;
        height: 30px;
        display: block;
    }
}
</style>