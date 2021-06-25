<template>
  <div class="search">
    <div class="container">
      <div class="search__inner" v-if="!pending">
        <input class="search__inner-input" v-model="searchValue" type="text" @click="putSearch" placeholder="Какие патроны вы ищите?">
        <div class="search__inner-title" >Ammu-Nation</div>
        <div class="total">
          <h3 class="search__total" :class="{ ammoNull }" @click="openCar(ammoPrice)"> Total : &euro;{{ammoPrice}} <div class="search__total-buy">Купить</div></h3>
          <div class="search__total-empty" v-if="ammoNull == true">Корзина пустая</div>
          <div v-if="openPopup == true">
            <div class="popup">
              <h3>Ammu-Nation</h3>
                <form class="popup__wrapper" @submit.prevent="submit">
                  
                  <div class="form__item form-group" :class="{ 'form-group--error': $v.formAmmo.name.value.$error }">
                    <input class="popup__wrapper-data" v-model="formAmmo.name.value" type="name" :placeholder="formAmmo.name.placeholder"/>
                    <div class="error" v-if="!$v.formAmmo.name.value.required && $v.formAmmo.name.value.$dirty">
                      Поле, обязательное для заполнения
                    </div>
                    <div class="error" v-if="!$v.formAmmo.name.value.minLength && $v.formAmmo.name.value.$dirty">
                      Имя должно иметь не менее
                      {{ $v.formAmmo.name.value.$params.minLength.min }} символа.
                    </div>
                  </div>

                  <div class="form__item form-group" :class="{ 'form-group--error': $v.formAmmo.surname.value.$error }">
                    <input class="popup__wrapper-data" v-model="formAmmo.surname.value" type="name" :placeholder="formAmmo.surname.placeholder">
                    <div class="error" v-if="!$v.formAmmo.surname.value.required && $v.formAmmo.surname.value.$dirty">
                      Поле, обязательное для заполнения
                    </div>
                    <div class="error" v-if="!$v.formAmmo.surname.value.minLength && $v.formAmmo.surname.value.$dirty">
                      Имя должно иметь не менее
                      {{ $v.formAmmo.surname.value.$params.minLength.min }} символа.
                    </div>
                  </div>

                  <div class="form__item form-group" :class="{ 'form-group--error': $v.formAmmo.tel.value.$error }">
                    <input class="popup__wrapper-data" v-model="formAmmo.tel.value" type="tel" :placeholder="formAmmo.tel.placeholder">
                    <div class="error" v-if="!$v.formAmmo.tel.value.required && $v.formAmmo.tel.value.$dirty">
                      Поле, обязательное для заполнения
                    </div>
                  </div>

                  <div class="form__item form-group" :class="{ 'form-group--error': $v.formAmmo.email.value.$error }">
                    <input class="popup__wrapper-data" v-model="formAmmo.email.value" type="email" :placeholder="formAmmo.email.placeholder">
                    <div class="error" v-if="!$v.formAmmo.email.value.required && $v.formAmmo.email.value.$dirty">
                      Поле, обязательное для заполнения
                    </div>
                    <div class="error" v-if="!$v.formAmmo.email.value.email && $v.formAmmo.email.value.$dirty">
                      Введите корректный Email
                    </div>
                  </div>

                  <div class="form__inner">
                    <p class="form__info form__info-valid" v-if="submitStatus === 'OK'">Спасибо за заявку!</p>
                    <p class="form__info form__info-error" v-if="submitStatus === 'ERROR'">Пожалуйста, заполните форму правильно.</p>
                    <p class="form__info form__info-pending" v-if="submitStatus === 'PENDING'">Загрузка...</p>
                    <button class="button" type="submit" :disabled="submitStatus === 'PENDING'">Отправить</button>
                  </div>
                </form>
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
import { required, minLength, email } from 'vuelidate/lib/validators'
export default {
  name: "SearchAmmo",
  data: function () {
      return {
          pending: true,
          searchData: [],
          searchValue: '',
          cardSumm: [],
          num: 1,
          ammoPrice: '0',
          openPopup: false,
          formAmmo: {},
          submitStatus: null,
          ammoNull: false
      };
  },
  validations: {
    formAmmo: {
      name: {
        value: {
          required,
          minLength: minLength(4)
        }
      },
      surname: {
        value: {
          required,
          minLength: minLength(4)
        }
      },
      tel: {
        value: {
          required,
        }
      },
      email: {
        value: {
          required,
          email
        }
      }
    }
  },
  computed: {
    sortSearchItem: function () {
        return this.searchData.filter((Search) => {
            return Search.title.toLowerCase().match(this.searchValue.toLowerCase())
        });
    },
  },
  created: function () {
     this.axios.get("./static/formAmmo.json").then((response) => {
      this.formAmmo = response.data;
      this.pending = false;
    });
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
      this.ammoNull = false;
    },
    numProductTake: function(add) {
      if (add.numProduct !== 0) {
        add.numProduct = Number(add.numProduct--) - this.num;
        this.ammoPrice =  Number(this.ammoPrice) - Number(add.price); 
      }
    },
    openCar: function (open) {
      if (open != 0) {
        this.openPopup = true;
        this.ammoNull = false;
      } else {
        console.log("корзина пустая");
        this.ammoNull = true;
      }
    },
    submit() {
      console.log('submit!')
      this.$v.$touch()
      if (this.$v.$invalid) {
        this.submitStatus = 'ERROR'
      } else {
        this.submitStatus = 'PENDING'
        setTimeout(() => {
          this.submitStatus = 'OK'
          this.axios
            .post("/request", [this.formAmmo, "Общая цена "+this.ammoPrice, this.sortSearchItem])
            console.log('успешно!')
            .then(function (response) {
              console.log(response)
            })
            .catch(function (error) {
              console.log(error)
          });
        }, 500)
      }
    }
  },
};
</script>
<style scoped lang="scss">
.button {
    display: inline-block;
    font-size: 23px;
    font-weight: bold;
    color: #ffffff;
    position: relative;
    padding-left: 35px;
    background-color: #000;
    padding: 10px 20px;
    border-radius: 5px;
    margin-top: 5px;
}

.ammoNull {
  color: red !important;
  border: 1px solid red !important;
}

.error {
  font-size: 10px;
  color: red;
}

.form__inner {
  display: block;
  width: 100%;
}
.form__info {
  font-size: 15px;
  display: block;
  text-align: center;
  &-error {
    color: red;
  }
  &-valid {
    color: green;
  }
  &-pending {
    color:orange;
  }
}
.search {
  position: relative;
  margin-bottom: 50px;
    .popup {
      width: 500px;
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
        width: 100%;
        .form__item {
          width: calc(50% - 20px);
          margin: 0 10px 10px;
        }
        &-data {
          width: 100%;
          height: 30px;
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
    .total {
      position: relative;
    }
    &__total {
      border: 1px solid #000;
      padding: 10px 20px 15px;
      border-radius: 5px;
      cursor: pointer;
      position: relative;
      &-buy {
        position: absolute;
        font-size: 9px;
        bottom: 5px;
        left: 50%;
        transform: translateX(-50%);
        text-transform: uppercase;
      }
      &-empty {
        position: absolute;
        bottom: 0px;
        font-size: 9px;
        left: 50%;
        transform: translateX(-50%);
        white-space: nowrap;
        color: red;
        text-transform: uppercase;
      }
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
