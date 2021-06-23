<template>
  <div class="search" @click="resetSearch">
    <div class="container">
      <div class="search__inner">
        <input class="search__inner-input" v-model="searchValue" type="text" @click="putSearch" placeholder="Какие патроны вы ищите?">
        <div class="search__inner-title">Ammu-Nation</div>
      </div>
      <ul class="search__list" v-for="item in sortSearchItem" :key="item.id">
          <li class="search__item">
              <div class="search__item-title">{{item.title}}</div>
              <div class="search__item-text">{{item.text}}</div>
              <div class="search__item-btn">{{item.submit}}</div>
          </li>
      </ul>
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
          basketSearch: []
      };
  },
  computed: {
    sortSearchItem: function () {
        return this.searchData.filter((Search) => {
            return Search.title.toLowerCase().match(this.searchValue.toLowerCase())
        });
    }
  },
  methods: {
    putSearch: function(){
        this.axios.get("./static/search.json")
        .then((response) => {
            this.searchData = response.data;
        });
    },
    resetSearch: function() {
      this.searchData = []
    },
  },
};
</script>
<style scoped lang="scss">
.search {
  margin-bottom: 50px;
    &__inner {
        display: flex;
        justify-content: space-around;
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
          padding-left: 15px;
          cursor: pointer;
          flex-shrink: 0;
          margin-left: 10px;
          &::before {
            content: '▲';
            position: absolute;
            left: 13px;
            top: 50%;
            transform: translateY(-50%);
          }
        }
    }   
}
</style>
