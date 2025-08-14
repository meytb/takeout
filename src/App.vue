<script setup lang="ts">
import { ref, computed, nextTick, watch } from 'vue'
import typeImage from './assets/type.jpeg'

const active = ref(0);
const titleActve = ref(0); // 侧边栏选中项索引
const show = ref(false);
const shopShow = ref(false);
// 获取右侧内容容器的引用
const sidebarContentWrapper = ref<HTMLElement | null>(null)
// 获取每个分类的引用
const categoryRefs = ref<HTMLElement[]>([])

// 完整的商品列表数据
const list = ref([
  {
    type: '鲜萃咖啡',
    val: [
      {
        name: '椰椰拿铁',
        price: 18.9,
        num: 0,
        img: typeImage,
        orderPrice: 24.9,
        sumNum: 15
      },
      {
        name: '拿铁',
        price: 10.9,
        num: 0,
        img: typeImage,
        orderPrice: 14.9,
        sumNum: 12
      },
      {
        name: '摩卡',
        price: 12.9,
        num: 0,
        img: typeImage,
        orderPrice: 19.9,
        sumNum: 2
      },
      {
        name: '卡布奇诺',
        price: 14.9,
        num: 0,
        img: typeImage,
        orderPrice: 24.9,
        sumNum: 6
      }
    ]
  },
  {
    type: '雪王新品',
    val: [
      {
        name: '雪王咖啡',
        price: 18.9,
        num: 0,
        img: typeImage,
        orderPrice: 24.9,
        sumNum: 18
      },
      {
        name: '雪王拿铁',
        price: 18.9,
        num: 0,
        img: typeImage,
        orderPrice: 24.9,
        sumNum: 22
      },
      {
        name: '雪王摩卡',
        price: 18.9,
        num: 0,
        img: typeImage,
        orderPrice: 24.9,
        sumNum: 32
      },
      {
        name: '雪王卡布奇诺',
        price: 18.9,
        num: 0,
        img: typeImage,
        orderPrice: 24.9,
        sumNum: 12
      }
    ]
  },
  {
    type: '清爽果茶',
    val: [
      {
        name: '柠檬蜂蜜茶',
        price: 12.0,
        num: 0,
        img: typeImage,
        orderPrice: 16.0,
        sumNum: 25
      },
      {
        name: '百香果茶',
        price: 13.0,
        num: 0,
        img: typeImage,
        orderPrice: 17.0,
        sumNum: 20
      },
      {
        name: '蜜桃乌龙茶',
        price: 11.0,
        num: 0,
        img: typeImage,
        orderPrice: 15.0,
        sumNum: 18
      },
      {
        name: '葡萄柚绿茶',
        price: 14.0,
        num: 0,
        img: typeImage,
        orderPrice: 18.0,
        sumNum: 15
      }
    ]
  },
  {
    type: '鲜冰淇淋',
    val: [
      {
        name: '香草冰淇淋',
        price: 8.0,
        num: 0,
        img: typeImage,
        orderPrice: 12.0,
        sumNum: 30
      },
      {
        name: '巧克力冰淇淋',
        price: 9.0,
        num: 0,
        img: typeImage,
        orderPrice: 13.0,
        sumNum: 28
      },
      {
        name: '草莓冰淇淋',
        price: 8.5,
        num: 0,
        img: typeImage,
        orderPrice: 12.5,
        sumNum: 22
      }
    ]
  },
  {
    type: '轻乳系列',
    val: [
      {
        name: '轻乳茉莉花茶',
        price: 13.0,
        num: 0,
        img: typeImage,
        orderPrice: 17.0,
        sumNum: 17
      },
      {
        name: '轻乳红茶',
        price: 12.5,
        num: 0,
        img: typeImage,
        orderPrice: 16.5,
        sumNum: 14
      },
      {
        name: '轻乳乌龙茶',
        price: 13.5,
        num: 0,
        img: typeImage,
        orderPrice: 17.5,
        sumNum: 12
      }
    ]
  },
  {
    type: '鲜煮奶茶',
    val: [
      {
        name: '珍珠奶茶',
        price: 11.0,
        num: 0,
        img: typeImage,
        orderPrice: 15.0,
        sumNum: 35
      },
      {
        name: '红豆奶茶',
        price: 12.0,
        num: 0,
        img: typeImage,
        orderPrice: 16.0,
        sumNum: 28
      },
      {
        name: '椰果奶茶',
        price: 11.5,
        num: 0,
        img: typeImage,
        orderPrice: 15.5,
        sumNum: 25
      }
    ]
  },
  {
    type: '纯茶奶盖',
    val: [
      {
        name: '奶盖绿茶',
        price: 14.0,
        num: 0,
        img: typeImage,
        orderPrice: 18.0,
        sumNum: 20
      },
      {
        name: '奶盖红茶',
        price: 14.0,
        num: 0,
        img: typeImage,
        orderPrice: 18.0,
        sumNum: 18
      },
      {
        name: '奶盖乌龙茶',
        price: 15.0,
        num: 0,
        img: typeImage,
        orderPrice: 19.0,
        sumNum: 15
      }
    ]
  },
  {
    type: '欢享套餐',
    val: [
      {
        name: '咖啡套餐A',
        price: 28.0,
        num: 0,
        img: typeImage,
        orderPrice: 35.0,
        sumNum: 12
      },
      {
        name: '咖啡套餐B',
        price: 32.0,
        num: 0,
        img: typeImage,
        orderPrice: 40.0,
        sumNum: 10
      }
    ]
  },
  {
    type: '加完小料',
    val: [
      {
        name: '珍珠',
        price: 2.0,
        num: 0,
        img: typeImage,
        orderPrice: 3.0,
        sumNum: 50
      },
      {
        name: '椰果',
        price: 2.0,
        num: 0,
        img: typeImage,
        orderPrice: 3.0,
        sumNum: 45
      },
      {
        name: '红豆',
        price: 2.5,
        num: 0,
        img: typeImage,
        orderPrice: 3.5,
        sumNum: 40
      },
      {
        name: '布丁',
        price: 3.0,
        num: 0,
        img: typeImage,
        orderPrice: 4.0,
        sumNum: 35
      }
    ]
  },
  {
    type: '零食小吃',
    val: [
      {
        name: '薯条',
        price: 8.0,
        num: 0,
        img: typeImage,
        orderPrice: 12.0,
        sumNum: 30
      },
      {
        name: '鸡块',
        price: 12.0,
        num: 0,
        img: typeImage,
        orderPrice: 16.0,
        sumNum: 25
      },
      {
        name: '洋葱圈',
        price: 10.0,
        num: 0,
        img: typeImage,
        orderPrice: 14.0,
        sumNum: 20
      }
    ]
  },
  {
    type: '欢享套餐', // 第二个欢享套餐
    val: [
      {
        name: '果茶套餐A',
        price: 25.0,
        num: 0,
        img: typeImage,
        orderPrice: 32.0,
        sumNum: 8
      },
      {
        name: '果茶套餐B',
        price: 29.0,
        num: 0,
        img: typeImage,
        orderPrice: 36.0,
        sumNum: 6
      }
    ]
  }
])

// 使用计算属性来动态计算总价
const sumPrice = computed(() => {
  let total = 0
  list.value.forEach(category => {
    category.val.forEach(item => {
      total += item.price * item.num
    })
  })
  return total.toFixed(2)
})

// 使用计算属性来动态计算总数量
const sumNum = computed(() => {
  let total = 0
  list.value.forEach(category => {
    category.val.forEach(item => {
      total += item.num
    })
  })
  return total
})

watch(sumNum, (newVal) => {
  shopShow.value = newVal > 0

  // 如果购物车变为空且popup正在显示，则关闭popup
  if (newVal === 0 && show.value) {
    show.value = false
  }
}, { immediate: true })

// 添加商品数量
const add = (categoryIndex: number, itemIndex: number) => {
  list.value[categoryIndex].val[itemIndex].num += 1
}

// 处理侧边栏点击事件，滚动到对应分类
const handleSidebarChange = (index: number) => {
  titleActve.value = index
  scrollToCategory(index)
}

// 滚动到指定分类
const scrollToCategory = (index: number) => {
  nextTick(() => {
    if (categoryRefs.value[index]) {
      categoryRefs.value[index].scrollIntoView({
        behavior: 'smooth',
        block: 'start'
      })
    }
  })
}
const clearCart = () => {
  // 遍历所有商品，将数量设置为0
    list.value.forEach(category => {
      category.val.forEach(item => {
        item.num = 0
      })
    })
    // 清空后自动关闭弹窗
    show.value = false
}
</script>

<template>
  <div class="container">
    <div class="header">
      <img class="logo" src="./assets/images.png" alt="">
      <div>
        <h3>蜜雪冰城</h3>
        <h5>美团外卖，送啥都快</h5>
      </div>
    </div>
    <div class="content">
      <van-tabs v-model:active="active">
        <van-tab title="点菜">
          <div class="sidebar-title">
            <!-- 为 van-sidebar 添加类名以便设置样式 -->
            <van-sidebar v-model="titleActve" class="custom-sidebar" @change="handleSidebarChange">
              <van-sidebar-item v-for="(category, index) in list" :key="index" :title="category.type"
                :dot="index === 0" />
            </van-sidebar>
            <!-- 修改内容区域结构，显示所有分类内容 -->
            <div class="sidebar-content-wrapper" ref="sidebarContentWrapper">
              <div class="sidebar-content">
                <!-- 显示所有分类的内容 -->
                <div class="sidebar-content-item" v-for="(val, index) in list" :key="index"
                  :ref="(el) => { if (el) categoryRefs[index] = el as HTMLElement }">
                  <h5>{{ val.type }}</h5>
                  <div class="sidebar-items-container">
                    <div class="sidebar-item" v-for="(item, idx) in val.val" :key="idx">
                      <van-card :title="item.name" :desc="'月售' + item.sumNum" :price="item.price"
                        :origin-price="item.orderPrice" :thumb="typeImage">
                        <template #tags>
                          <div class="card-tags">
                            <div>
                              <van-tag style="margin-right: 6px;" class="tag" plain type="primary">1杯</van-tag>
                              <van-tag class="tag" plain type="primary">咖啡</van-tag>
                            </div>

                            <!-- 修复逻辑：仅根据 item.num 判断显示哪个组件 -->
                            <van-icon v-if="item.num === 0" @click="add(index, idx)" name="add" color="#1989fa"
                              size="25" />
                            <van-stepper v-else v-model="item.num" min="0" theme="round" button-size="22"
                              disable-input />
                          </div>
                        </template>
                      </van-card>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </van-tab>
        <van-tab title="超优惠">内容 2</van-tab>
        <van-tab title="评价">内容 3</van-tab>
        <van-tab title="商家">内容 4</van-tab>
      </van-tabs>
    </div>
    <div class="footer" v-if="shopShow">
      <div style="display: flex; align-items: center;">
        <van-badge :content="sumNum" max="99">
          <img class="ai-logo" @click="show = true" src="./assets/waimai.png" alt="">
        </van-badge>
        <div style="margin-left: 12px;">
          <span class="price">到手约为 <span style="font-size: 20px;">¥{{ sumPrice }}</span></span><br>
          <span
            style="font-size: 14px;font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;color: #999;">免配送费</span>
        </div>
      </div>
      <div class="btn">去结算</div>
    </div>
    <van-popup :overlay-style="{ height: '89%', top: 0, zIndex: 998 }" v-model:show="show" position="bottom" :style="{
      height: '50%',
      borderTopLeftRadius: '12px',
      borderTopRightRadius: '12px',
      bottom: '11%',
      zIndex: 999,
      backgroundColor: '#fff'
    }">
      <div class="popup-content">
        <van-nav-bar title="购物车" left-text="清空" right-text="" @click-left="clearCart" @click-right="show = false"
          :left-disabled="sumNum === 0">
          <template #right>
            <van-icon name="cross" size="20" />
          </template>
        </van-nav-bar>
        <div class="cart-items" style="padding: 16px; max-height: calc(100% - 60px); overflow-y: auto;">
          <div v-for="(category, categoryIndex) in list" :key="categoryIndex">
            <div v-for="(item, itemIndex) in category.val" :key="itemIndex" v-show="item.num > 0" class="cart-item"
              style="display: flex; justify-content: space-between; align-items: center; padding: 12px 0; border-bottom: 1px solid #f5f5f5;">
              <!-- 商品图片和信息 -->
              <div style="display: flex; align-items: center; flex: 1;">
                <img :src="item.img" alt="" style="width: 60px; height: 60px; border-radius: 8px; margin-right: 12px;">
                <div>
                  <div style="font-weight: 500;">{{ item.name }}</div>
                  <div style="font-size: 14px; color: #999;">¥{{ item.price }}</div>
                </div>
              </div>
              <!-- 步进器 -->
              <van-stepper v-model="item.num" min="0" theme="round" button-size="22" disable-input />
            </div>
          </div>
        </div>
      </div>
    </van-popup>
  </div>
</template>

<style scoped>
.container {
  padding: 12px;
  width: 100%;
  min-height: 100vh;
  box-sizing: border-box;
  background-color: #f5f5f5;
}

.header {
  display: flex;
  align-items: center;
  background-color: #fff;
  border-radius: 12px;
  padding: 12px 24px;
  box-sizing: border-box;
}

.logo {
  width: 100px;
  height: 100px;
  border-radius: 12px;
  margin-right: 24px;
}

.ai-logo {
  width: 75px;
  height: 75px;
}

.content {
  background-color: #fff;
  border-radius: 12px;
  padding: 12px 12px;
  box-sizing: border-box;
  margin-top: 36px;
}

.sidebar-title {
  display: flex;
}

/* 新增包装容器，用于控制整体高度和滚动 */
.sidebar-content-wrapper {
  flex: 1;
  max-height: 592px;
  overflow-y: auto;
  scrollbar-width: none;
  /* Firefox */
  -ms-overflow-style: none;
  /* IE 10+ */
}

.sidebar-content-wrapper::-webkit-scrollbar {
  width: 0px;
  background: transparent;
}

.sidebar-content {
  width: 100%;
}

/* 为侧边栏添加滚动样式 */
.custom-sidebar {
  max-height: 592px;
  overflow-y: auto;
  width: 81px;
  flex-shrink: 0;
  margin-right: 20px;
  scrollbar-width: none;
}

/* 隐藏 Webkit 浏览器的滚动条 */
.custom-sidebar::-webkit-scrollbar {
  width: 0px;
  background: transparent;
}

.sidebar-items-container {
  max-height: 610px;
  overflow-y: auto;
  /* 隐藏滚动条但保持滚动功能 */
  scrollbar-width: none;
  /* Firefox */
  -ms-overflow-style: none;
  /* IE 10+ */
}

.sidebar-items-container::-webkit-scrollbar {
  width: 0px;
  /* Chrome, Safari, Opera */
  background: transparent;
}

.sidebar-item {
  margin-bottom: 12px;
}

.card-tags {
  display: flex;
  align-items: center;
  gap: 8px;
  justify-content: space-between
}

.tag {
  font-size: 10px;
}

.footer {
  background-color: #fff;
  padding: 12px 0;
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  justify-content: space-around;
  align-items: center;
  z-index: 1000;
  /* 确保footer在最上层 */
}

.price {
  color: red;
  font-size: 14px;
  font-weight: 600;
  font-family: 'Courier New', Courier, monospace;
}

.btn {
  width: 100px;
  height: 35px;
  background-color: rgba(255, 255, 0, 0.78);
  color: black;
  border-radius: 12px;
  text-align: center;
  line-height: 35px;
}
</style>