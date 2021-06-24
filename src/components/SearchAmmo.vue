<template>
  <div class="search">
    <div class="container">
      <div class="search__inner">
        <input class="search__inner-input" v-model="searchValue" type="text" @click="putSearch" placeholder="Какие патроны вы ищите?">
        <div class="search__inner-title">Ammu-Nation</div>
        <div class="total">
          <h3> Total : &euro;{{ammoPrice}} </h3>
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
export default {
  name: "SearchAmmo",
  data: function () {
      return {
          searchData: [],
          searchValue: '',
          cardSumm: [],
          num: 1,
          ammoPrice: ''
      };
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
      if (this.ammoPrice > 0) {
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
