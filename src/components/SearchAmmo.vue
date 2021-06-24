<template>
  <div class="search">
    <div class="container">
      <div class="search__inner">
        <input class="search__inner-input" v-model="searchValue" type="text" @click="putSearch" placeholder="Какие патроны вы ищите?">
        <div class="search__inner-title" >Ammu-Nation</div>
        <div class="total">
          <h3 class="search__total" @click="openPopup = true"> Total : &euro;{{ammoPrice}} </h3>
          <div v-if="openPopup == true">
            <div class="popup">
               <h3>Ammu-Nation</h3>
               <div class="popup__wrapper" >
                  <input class="popup__wrapper-data" type="text" placeholder="Ваше имя"/>
                  <input class="popup__wrapper-data" type="text" placeholder="Ваша фамилия">
                  <input class="popup__wrapper-data" type="tel" placeholder="Ваше телефон">
                  <input class="popup__wrapper-data" type="email" placeholder="Ваш email">
                  <div @click="sendAmmo(ammoPrice)">Отправить</div>
               </div>
            </div>
            <div class="popup-blackout" @click="openPopup = false"></div>
          </div>
        </div>
      </div>
      <ul class="search__list" v-for="item in sortSearchItem" :key="item.id">
          <li class="search__item">
              <div class="search__item-title">{{item.title}}</div>
              <div class="search__item-text">{{item.text}}</div>
              <div class="search__item-number"><div @click="numProductTake(item)">-</div><div>{{item.numProduct}}</div><div @click="numProductAdd(item)">+</div></div>
          </li>  
      </ul>
      <div class="search__close" v-if="searchData.length > 0" @click="closeSearch">Закрыть</div>
    </div>
  </div>
</template>
<script>
import { required, minLength } from 'vuelidate/lib/validators'
export default {
  name: "SearchAmmo",
  data: function () {
      return {
          searchData: [],
          searchValue: '',
          cardSumm: [],
          num: 1,
          ammoPrice: '0',
          openPopup: false,
          name: ''
      };
  },
  validations: {
    name: {
      required,
      minLength: minLength(4)
    },
  },
  computed: {
    sortSearchItem: function () {
        return this.searchData.filter((Search) => {
            return Search.title.toLowerCase().match(this.searchValue.toLowerCase())
        });
    },
  },
  methods: {
    putSearch: function(){
        this.axios.get("./static/search.json")
        .then((response) => {
            this.searchData = response.data;
        });
    },
    sendAmmo: function(Price) {
      console.log(Price)
    },
    closeSearch: function() {
      this.searchData = []
    },
    cardSearch: function(cardId) {
      this.cardSumm.push(cardId);
      console.log(this.cardSumm)
    },
    numProductAdd: function(add) {
      add.numProduct = Number(add.numProduct++) + this.num;
      this.ammoPrice =  Number(this.ammoPrice) + Number(add.price);
    },
    numProductTake: function(add) {
      if (add.numProduct !== 0) {
        add.numProduct = Number(add.numProduct--) - this.num;
        this.ammoPrice =  Number(this.ammoPrice) - Number(add.price); 
      }
    }
  },
};
</script>
<style scoped lang="scss">
.search {
  position: relative;
  margin-bottom: 50px;
    .popup {
      max-width: 500px;
      background-color: #fff;
      position: fixed;
      left: 50%;
      top: 50%;
      transform: translate(-50%, -50%);
      border-radius: 5px;
      padding: 50px 20px;
      z-index: 10;
      &-blackout {
        position: fixed;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background-color: rgba(0, 0, 0, 0.7);
        z-index: 8;
      }
      &__wrapper {
        display: flex;
        justify-content: space-between;
        flex-wrap: wrap;
        &-data {
          width: calc(50% - 15px);
          height: 30px;
          margin-bottom: 20px;
        }
      }
    }
    &__close {
      display: inline-block;
      font-size: 23px;
      font-weight: bold;
      color: #ffffff;
      position: relative;
      padding-left: 35px;
      background-color: #000;
      padding: 10px 20px;
      border-radius: 5px;
    }
    &__total {
      border: 1px solid #000;
      padding: 10px 20px;
      border-radius: 5px;
      cursor: pointer;
    }
    &__inner {
        display: flex;
        justify-content: space-around;
        align-items: center;
        &-input {
            width: 300px;
            height: 30px;
            position: relative;
            display: block;
            padding: 0 10px;
            border-radius: 3px;
        }

        &-title {
            font-size: 30px;
            font-weight: bold;
            color: #D6A74B;
            position: relative;
            padding-left: 35px;
            &::before {
              content: '★';
              position: absolute;
              top: 50%;
              left: 0;
              transform: translateY(-50%);
            }
        }
    }
    
    &__list {
        padding: 0;
        margin: 20px;
    }
    &__item {
        display: flex;
        justify-content: center;
        list-style-type: none; 
        min-height: 50px;
        padding: 10px 20px;
        border-bottom: 1px solid #000;
        cursor: pointer;
        &-title {
            font-size: 20px;
            font-weight: bold;
            flex-shrink: 0;
            margin-right: 10px;
        }
        &-text {
            font-size: 16px;
        }

        &-number {
          display: flex;
          justify-content: space-between;
          align-items: center;
          flex-shrink: 0;
          width: 40px;
        }

        &-btn {
          display: flex;
          justify-content: center;
          align-items: center;
          width: 100px;
          height: 50px;
          color: #fff;
          margin: 0 auto;
          border-radius: 10px;
          background-color: rgb(0, 0, 0);
          position: relative;
          cursor: pointer;
          flex-shrink: 0;
          margin-left: 10px;
        }
    }   
}
</style>
