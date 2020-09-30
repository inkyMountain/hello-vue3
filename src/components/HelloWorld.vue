<template>
  <div class="goods-list">
    <div class="goods" v-for="goods in goodsList" :key="goods.id">
      {{ goods.name }}
    </div>
  </div>
</template>

<script>
import { onMounted, reactive, watch } from 'vue'

export default {
  name: 'HelloWorld',

  setup(props) {
    const fetchGoodsList = () => {
      return new Promise((resolve) => {
        setTimeout(() => {
          const goodsList = [...new Array(10)].map((_, index) => {
            return {
              id: index,
              name: `Goods${index}`,
            }
          })
          resolve(goodsList)
        }, 1000)
      })
    }

    const goodsList = reactive([])

    watch(
      goodsList,
      (list, oldList) => {
        console.log('list', list)
        console.log('oldList', oldList)
      },
      {
        immediate: true,
      }
    )

    onMounted(async () => {
      goodsList.push(...(await fetchGoodsList()))
    })

    return {
      goodsList,
      fetchGoodsList,
    }
  },

  props: {
    msg: String,
  },
}
</script>

<style lang="less" scoped>
.count {
  background-color: lightblue;
}
</style>
