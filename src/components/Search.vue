<template>
  <div class="search">
    <div class="container">
      <div class="search__inner">
        <input class="search__inner-input" v-model="searchValue" type="text" @click="putSearch" placeholder="Что вы ищите?">
        <div class="search__inner-title">Оружие</div>
      </div>
      <ul class="search__list" v-for="item in sortSearchItem" :key="item.id">
          <li class="search__item">
              <div class="search__item-title">{{item.title}}</div>
              <div class="search__item-text">{{item.text}}</div>
              <div>{{resultSearch}}</div>
          </li>
      </ul>
    </div>
  </div>
</template>
<script>
export default {
  name: "Search",
  data: function () {
      return {
          searchData: [],
          searchValue: '',
          resultSearch: ''
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
            console.log(this.searchData)
        });
    },
  },
};
</script>
<style scoped lang="scss">
.container {
  max-width: 920px;
  margin: 0 auto;
  border: 1px solid #000;
  border-bottom: none;
  padding: 50px 0;
}

.search {
    .search__inner {
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
        }
        &-text {
            font-size: 16px;
        }
    }   
}
</style>
