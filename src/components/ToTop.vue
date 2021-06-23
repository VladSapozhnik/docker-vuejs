<template>
    <div class="container">
      <div class="toTop" @click="toTop">на верх</div>
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
    }
  },
  methods: {
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
  cursor: pointer;
  &::before {
    content: '▲';
    position: absolute;
    left: 13px;
    top: 50%;
    transform: translateY(-50%);
  }
}

</style>
