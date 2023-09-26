<script setup>
import { ref, reactive } from 'vue'

// 食物列表
const foodList = ref
const meatListData = reactive([
  { food: "牛肉片", time: "60" },
  { food: "羊肉片", time: "60" },
  { food: "鸡肉片", time: "90" },
  { food: "牛蛙", time: "180" },
  { food: "牛百叶", time: "120" },
  { food: "牛骨髓", time: "300" },
  { food: "牛肚", time: "180" },
  { food: "牛黄喉", time: "180" },
  { food: "特色虾滑", time: "120" },
  { food: "鲍鱼片", time: "60" },
  { food: "撒尿牛丸", time: "120" },
  { food: "肉脆皮", time: "90" },
  { food: "德庄鲜鸭血", time: "600" },
  { food: "德庄嫩牛肉", time: "60" },
  { food: "德庄酱香肉片", time: "60" },
  { food: "鲜猪黄喉", time: "180" },
  { food: "鲜猪脑花", time: "900" },
  { food: "功夫腰片", time: "60" },
  { food: "德庄酥肉", time: "90" },
  { food: "德庄千层肚", time: "180" },
  { food: "德庄挂面", time: "300" },
  { food: "鸭肠", time: "15" },
  { food: "鸡翅中", time: "300" },
  { food: "无骨鸭掌", time: "300" }
])
const seafoodListData = reactive([
  { food: "虾", time: "120" },
  { food: "蟹腿", time: "180" },
  { food: "鱼丸", time: "120" },
  { food: "鱼片", time: "90" },
  { food: "蟹仔丸", time: "120" },
  { food: "毛管鱼", time: "90" },
  { food: "龙利鱼", time: "90" },
  { food: "鲜虾", time: "120" },
  { food: "蛎子", time: "180" }
])
const vegetableListData = reactive([
  { food: "蘑菇", time: "180" },
  { food: "黄豆芽", time: "60" },
  { food: "白菜", time: "60" },
  { food: "沙拉菜", time: "60" },
  { food: "笋", time: "60" }
])
const beanListData = reactive([
  { food: "豆腐", time: "60" },
  { food: "家乡老豆腐", time: "60" },
  { food: "黄豆金腐", time: "60" },
  { food: "竹豆油皮", time: "60" }
])
const mainfoodListData = reactive([])

const fungiListData = reactive([])

const waitList = reactive([])


const deleteWaitList = (item) => {
  waitList.pop(item)
  console.log('-1')

}

const addWaitList = (item) => {
  // 克隆 item 对象
  const newItem = { ...item, id: waitList.length + 1 };
  waitList.push(newItem)
  // 倒计时
  countdown(newItem);
}

const countdown = (item) => {
  // 延时器
  const timer = setInterval(() => {
    item.time--
    console.log(item.food, item.time);

    if (item.time <= 0) {
      clearInterval(timer); // 如果时间为零或负数，清除定时器
    }
  }, 1000);
}

// 获取当前时间
const getNowTime = () => {
  const now = new Date();
  const minute = now.getMinutes();
  const second = now.getSeconds();
  const format_minute = minute < 10 ? '0' + minute : minute
  const format_second = second < 10 ? '0' + second : second
  return `${format_minute}m:${format_second}s`;
}
</script>

<template>
  <div class="container">
    <h1>涮火锅计时器</h1>

    <div class="content_container">
      <div class="content">
        <h2 class="sort">肉类：</h2>
        <div class="food" v-for="(item, index) in meatListData" :key="index" @click="addWaitList(item)">{{ item.food }}
        </div>
      </div>

      <div class="content">
        <h2 class="sort">河海鲜：</h2>
        <div class="food" v-for="(item, index) in seafoodListData" :key="index" @click="addWaitList(item)">{{ item.food }}
        </div>
      </div>

      <div class="content">
        <h2 class="sort">蔬菜：</h2>
        <div class="food" v-for="(item, index) in vegetableListData" :key="index" @click="addWaitList(item)">{{ item.food
        }}</div>
      </div>

      <div class="content">
        <h2 class="sort">豆制类：</h2>
        <div class="food" v-for="(item, index) in beanListData" :key="index" @click="addWaitList(item)">{{ item.food }}
        </div>
      </div>

      <div class="content">
        <h2 class="sort">主食：</h2>
        <div class="food" v-for="(item, index) in mainfoodListData" :key="index" @click="addWaitList(item)">{{ item.food
        }}</div>
      </div>

      <div class="content">
        <h2 class="sort">菌类：</h2>
        <div class="food" v-for="(item, index) in fungiListData" :key="index" @click="addWaitList(item)">{{ item.food }}
        </div>
      </div>
    </div>


    <div class="title">计时表</div>
    <div class="show">
      <div class="show_item" v-for="(item, index) in waitList" :key="index">
        <p>{{ item.id }}.{{ item.food }}({{ getNowTime() }}下锅):{{ item.time }}s后可吃!</p>
        <button @click="deleteWaitList(item)">删除</button>
      </div>
    </div>

  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100vw;
  height: 100vh;
  box-sizing: border-box;
}

.content_container {
  height: 600px;
  overflow: auto;
}

.container h1 {
  color: #812228;
  margin: 20px;
}

.content {
  display: flex;
  flex-wrap: wrap;
  justify-content: flex-start;
  align-items: center;
  align-content: center;
  padding: 10px;
}

.content h2 {
  color: #812228;

}

.food {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  align-content: center;
  width: auto;
  height: 32px;
  background-color: #fef0f0;
  color: #af6f73;
  margin: 2px 5px;
  padding: 5px;
  border: #af6f73 1px solid;
  border-radius: 6px;
  box-sizing: border-box;
}

.title {
  color: #812228;
  font-size: 22px;
  font-weight: 900;
}

.show {
  display: flex;
  flex: 1;
  justify-content: space-between;
  flex-wrap: wrap;
  min-height: 200px;
  /* 设置最小高度 */
  background-color: #ffffff;
  color: #af6f73;
  margin: 10px 20px;
  padding: 10px;
  border: #af6f73 1px solid;
  border-radius: 6px;
  overflow: auto;
  /* 如果内容溢出，显示滚动条 */
  box-sizing: border-box;
}

.show_item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
}
</style>
