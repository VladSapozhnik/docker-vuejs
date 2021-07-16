<template>
    <div class="popup" >
        <div class="container">
            <div class="menu" v-if="!pending">
                <div class="menu-title">Главная</div>
                <ul class="menu__inner">
                    <li @click="$event.target.classList.toggle('active')" v-for="item in menuData.list" :key="item.id"> 
                        <div class="menu__item"><a :href="item.href">{{item.title}}</a>
                            <div class="arrow"></div>
                            <ul>
                                <li v-for="item in item.item" class="menu__item-list" @click="$event.target.classList.toggle('open')" :key="item.id">
                                    <div class="menu__item-result"><a :href="item.href">{{item.title}}</a>
                                        <div class="arrow-item"></div>
                                        <ul class="menu__item-result--list">
                                            <li v-for="item in item.item" :key="item.id">
                                                <a :href="item.href">{{item.title}}</a>
                                            </li>
                                        </ul>
                                    </div> 
                                </li>
                            </ul>
                        </div>
                    </li>
                </ul>
            </div>
            <slot></slot>
            <div class="popup__head">Услуги</div>    
            <ul class="popup__head" v-for="item in item" :key="item.id">
                <li :id="item.id" @click="currentIndex = item.id" :class="{someClass: item.id===currentIndex}" class="head">{{item.title}}
                    <ul class="head-list" :id="item.id" @click="currentIndexItem = item.id" :class="{someClass: item.id===currentIndexItem}" v-for="item in item.item" :key="item.id">
                        <li>
                            <div>-->{{item.title}}</div>
                        </li>
                    </ul>
                </li>
            </ul>
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
    data: function () {
      return {
        pending: true,
        menuData: {},
        item: [],
        activeItem: null,
        currentIndex: '',
        currentIndexItem: ''
      };
    },
    created: function () {
        this.axios.get("./static/menu.json").then((response) => {
            this.menuData = response.data;
            this.pending = false;
        });
        this.axios.get("./static/menu-copy.json").then((response) => {
            this.item = response.data;
            this.pending = false;
        });
    },
    methods: {
        popupShow: function (e) {
            console.log(e);
            console.log(e.title)
        },
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
        width: 100%;
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
        width: max-content;
    }

    .menu-title {
        padding: 5px 10px;
        border: 1px solid green;
        display: block;
        width: 140px;
        margin-bottom: 5px;
    }
}

.menu__item {
    position: relative;
}

.arrow,
.arrow-item {
    display: block;
    width: 15px;
    height: 15px;
    position: absolute;
    background-image: url(/arrow.svg);
    background-repeat: no-repeat;
    background-position: center;
    background-size: contain;
    right: 30px;
    top: 15px;
}

.menu__item-list,
.menu__item-result--list {
    display: none;
}

.menu__item-result {
    position: relative;
}

.menu__item.active {
    .menu__item-list {
        display: block;
    }

    .arrow {
        transform: rotate(90deg);
    }
}

.menu__item-result.active {
    .menu__item-result--list {
        display: block;
    }

    .arrow-item {
        transform: rotate(90deg);
    }
}

.head-list {
    display: none;
}

.head.someClass {
    color: green;

    .head-list{
        display: block;
    }
}
.head-list.someClass {
    color: #000;
}
</style>