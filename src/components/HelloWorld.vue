<template>
  <div class="tabs">
    <template v-for="tab in tabs" :key="tab.id">
      <div class="tab">
        {{ tab.name }}
      </div>
    </template>
  </div>

  <div class="goods-list">
    <button @click="push()">push</button>

    <template v-for="goods in goodsList" :key="goods.id">
      <div class="goods">
        {{ goods.name }}
      </div>
    </template>

    <div class="goods-amount">
      {{ goodsAmount }}
    </div>
  </div>

  <Injected />
</template>

<script lang="ts">
import { computed, onMounted, reactive, watch, provide, inject } from "vue";
import useGoodsList from "../hooks/useGoodsList";
import useTabs from "../hooks/useTabs";
import Injected from './Injected.vue'

export default {
  name: "HelloWorld",

  components: {
    Injected,
  },

  setup(props) {
    const { goodsList, goodsAmount, fetchGoodsList, push } = useGoodsList();
    const { tabs, fetchTabs } = useTabs();

    onMounted(() => {
      fetchTabs()
        .then((fetchedTabs) => {
          tabs.push(...fetchedTabs);
          return fetchedTabs;
        })
        .then((fetchedTabs) => {
          const firstTab = fetchedTabs[0];
          return firstTab ? fetchGoodsList(firstTab.id.toString()) : [];
        })
        .then((fetchedGoodsList) => {
          goodsList.push(...fetchedGoodsList);
        });
    });

    return {
      goodsList,
      goodsAmount,
      fetchGoodsList,
      push,
      tabs,
      fetchTabs,
    };
  },

  props: {
    msg: String,
  },
};
</script>

<style lang="less" scoped>
.tabs {
  display: flex;
  align-items: center;
  justify-content: center;

  .tab:not(:last-child) {
    margin-right: 10px;
  }
}
</style>
