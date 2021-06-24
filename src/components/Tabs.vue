<template>
  <div class="tabs">
    <div class="container">
        <div class="tabs__items">
            <div class="tabs__item" @click="activetab=1" :class="[ activetab === 1 ? 'active' : '' ]">Рулетка</div>
            <div class="tabs__item" @click="activetab=2" :class="[ activetab === 2 ? 'active' : '' ]">Stack - очередь</div>
        </div>
        <div v-if="activetab === 1">
            <revolver-roll/>
        </div>
        <div v-if="activetab === 2">
            <hello-world/>
        </div>
    </div>
  </div>
</template>
<script>
import HelloWorld from './Stack.vue';
import RevolverRoll from './RevolverRoll.vue';
export default {
  components: { RevolverRoll, HelloWorld },
  name: "Tabs",
  data: function () {
      return {
          activetab: 1
      };
  },
  computed: {

  },
  methods: {
      tabRoll: function () {

      }
  },
};
</script>
<style scoped lang="scss">
.tabs {
    &__items {
        display: flex;
    }

    &__item {
        display: flex;
        align-items: center;
        justify-content: center;
        width: 150px;
        height: 30px;
        border: 1px solid #000;
        color: #fff;
        border-radius:1px;
        background-color: #000;
        cursor: pointer;
        & + & {
            margin-left: 5px;
        }
    }
}
</style>
