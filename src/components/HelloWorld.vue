<template>
  <div class="goods-list">
    <div class="goods" v-for="goods in goodsList" :key="goods.id">
      {{ goods.name }}
    </div>
  </div>

  <Injected />
</template>

<script>
import { onMounted, provide, reactive, watch } from 'vue'
import Injected from './Injected'

export default {
  name: 'HelloWorld',

  components: {
    Injected,
  },

  setup(props) {
    provide(
      'testProvide',
      reactive({
        value: 'value from provide',
      })
    )
    
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
