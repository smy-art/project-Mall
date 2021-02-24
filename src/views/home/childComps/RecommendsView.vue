<template>
  <div class="recommend">
    <div v-for="item in recommends"  class="recommend-item">
      <a :href="item.link">
          <img :src="item.image" alt="">
      </a>
      <div>{{item.title}}</div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    recommends: {
      type: Array,
      default() {
        return [];
      },
    },
  },
  components: {},
  data() {
    return {};
  },
};
</script>

<style scoped >
 .recommend {
    display: flex;
    margin-top: 10px;
    font-size: 12px;
    padding-bottom: 30px;
    border-bottom: 10px solid #eee;
  }

  .recommend-item {
    flex: 1;
    text-align: center;
  }

  .recommend img {
    width: 70px;
    height: 70px;
    margin-bottom: 10px;
  }
</style>
