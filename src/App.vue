<script setup>
import { ref, reactive, computed, onMounted, onUnmounted } from 'vue'

// 食物列表
const foodList = ref
const makeFood = ref('');
const makeTime = ref('');
const isShowText = ref(true)
const isShowTips = ref(true)
const scrollY = ref(0)
const isModalVisible = ref(false);

// const timeOut = ref(true)
const meatListData = ([
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
const seafoodListData = ([
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
const vegetableListData = ([
  { food: "蘑菇", time: "180" },
  { food: "黄豆芽", time: "60" },
  { food: "白菜", time: "60" },
  { food: "沙拉菜", time: "60" },
  { food: "笋", time: "60" }
])
const beanListData = ([
  { food: "豆腐", time: "60" },
  { food: "家乡老豆腐", time: "60" },
  { food: "黄豆金腐", time: "60" },
  { food: "竹豆油皮", time: "60" }
])
const mainfoodListData = ([{ food: "火锅面", time: "300" }, { food: "方便面", time: "180" }, { food: "红薯粉", time: "180" }, { food: "宽粉", time: "180" }, { food: "粉丝", time: "180" }, { food: "手擀面", time: "300" }, { food: "面筋", time: "300" }, { food: "饺子", time: "300" }])

const fungiListData = ([{ food: "金针菇", time: "180" }, { food: "平菇", time: "180" }, { food: "松茸", time: "180" }, { food: "鸡腿菇", time: "180" }, { food: "木耳", time: "180" }, { food: "香菇", time: "180" }, { food: "杏鲍菇", time: "180" }, { food: "杨枝菇", time: "180" }, { food: "云耳", time: "180" }, { food: "松蘑", time: "180" }])

const customList = reactive([])

const recordList = reactive([])

const waitList = reactive([])


// 监听鼠标下滑事件，下滑超过150px
const onScroll = (event) => {
  const timer = setInterval(() => {
    scrollY.value = event.target.scrollTop;
    if (scrollY.value > 50) {
      isShowText.value = false
      console.log('监听鼠标下滑事件，下滑超过150px', scrollY.value);
    }
    else if (scrollY.value < 50) {
      isShowText.value = true
      console.log('监听鼠标下滑事件，下滑小于150px', scrollY.value);
    }
    clearInterval(timer);
  }, 300);
}


// 获取当前时间
const getNowTime = () => {
  const now = new Date();
  const hour = now.getHours();
  const minute = now.getMinutes();
  const second = now.getSeconds();
  const format_hour = hour < 10 ? '0' + hour : hour
  const format_minute = minute < 10 ? '0' + minute : minute
  const format_second = second < 10 ? '0' + second : second
  return `${format_hour}时:${format_minute}分:${format_second}秒`;
}

const countdown = (item) => {
  // 延时器
  // console.log(item.value.status);

  const timer = setInterval(() => {
    item.value.time--
    // console.log(item.value.food, item.value.time);

    if (item.value.time <= 0) {
      clearInterval(timer); // 如果时间为零或负数，清除定时器
      item.value.status = false
      console.log(item);
      // 先删除数组该项再用unshift添加到数组第一项
      const index = waitList.indexOf(item)
      waitList.splice(index, 1)
      waitList.unshift(item)
    }
  }, 1000);
}

const deleteWaitList = (item) => {
  const index = waitList.indexOf(item)
  waitList.splice(index, 1)
  if (waitList.length == 0) {
    isShowTips.value = true
    // console.log('false', isShowTips.value, waitList.length);
  } else {
    isShowTips.value = false
    // console.log('true', isShowTips.value,waitList.length);
  }
}

const addWaitList = (item) => {
  if (waitList.length >= 0) {
    isShowTips.value = false
    console.log('true', isShowTips.value, waitList.length);
  } else {
    isShowTips.value = true
    console.log('false', isShowTips.value, waitList.length);
  }
  const nowTime = getNowTime()
  // 克隆 item 对象插入id，nowtime, status字段,并使对象变成响应式
  const newItem = ref({ ...item, id: waitList.length + 1, nowTime: nowTime, status: true })
  waitList.push(newItem)
  recordList.push(newItem)
  // 倒计时
  countdown(newItem);
}

// 提交历史食材
const recordWaitList = (item) => {
  const nowTime = getNowTime()
  // 克隆 item 对象插入id，nowtime, status字段,并使对象变成响应式
  const newItem = ref({ ...item, id: waitList.length + 1, nowTime: nowTime, status: true })
  waitList.push(newItem)
  console.log(waitList[0].value.status);

  // 倒计时
  countdown(newItem);
}

const showModal = () => {
  isModalVisible.value = true;
};

const closeModal = () => {
  isModalVisible.value = false;
};

// 提交自定义食材
const submitFood = (item) => {
  if (waitList.length >= 0) {
    isShowTips.value = false
    console.log('true', isShowTips.value, waitList.length);
  } else {
    isShowTips.value = true
    console.log('false', isShowTips.value, waitList.length);
  }
  if (makeFood.value && makeTime.value) {
    // event.preventDefault();
    const nowTime = getNowTime()
    const newItem = ref({ id: customList.length + 1, food: makeFood.value, time: makeTime.value, nowTime: nowTime, status: true })
    console.log(item);

    waitList.push(newItem)
    recordList.push(newItem)
    // 倒计时
    countdown(newItem);
    makeFood.value = '';
    makeTime.value = '';
  } else {
    // alert('请输入食材名称和时间')
    showModal();
    console.log('请输入食材名称和时间');
  }
}

// 数据持久化

</script>

<template>
  <div class="container">
    <h1>涮(shuan)火锅计时器🍲</h1>
    <p style="color: #c4c4c4;">{{ isShowText ? '------下滑查看更多食品------' : '扎布多德勒😋' }}</p>
    <div class="content_container" @scroll="onScroll">
      <div class="content">
        <h2 class="sort">肉类：</h2>
        <div class="food" v-for="(item, index) in meatListData" :key="index" @click="addWaitList(item)">{{ item.food }}:{{
          item.time }}s
        </div>
      </div>

      <div class="content">
        <h2 class="sort">河海鲜：</h2>
        <div class="food" v-for="(item, index) in seafoodListData" :key="index" @click="addWaitList(item)">{{ item.food
        }}:{{ item.time }}s
        </div>
      </div>

      <div class="content">
        <h2 class="sort">蔬菜：</h2>
        <div class="food" v-for="(item, index) in vegetableListData" :key="index" @click="addWaitList(item)">{{ item.food
        }}:{{ item.time }}s</div>
      </div>

      <div class="content">
        <h2 class="sort">豆制类：</h2>
        <div class="food" v-for="(item, index) in beanListData" :key="index" @click="addWaitList(item)">{{ item.food }}:{{
          item.time }}s
        </div>
      </div>

      <div class="content">
        <h2 class="sort">主食：</h2>
        <div class="food" v-for="(item, index) in mainfoodListData" :key="index" @click="addWaitList(item)">{{ item.food
        }}:{{ item.time }}s</div>
      </div>

      <div class="content">
        <h2 class="sort">菌类：</h2>
        <div class="food" v-for="(item, index) in fungiListData" :key="index" @click="addWaitList(item)">{{ item.food
        }}:{{ item.time }}s
        </div>
      </div>

      <!-- <div class="content">
        <h2 class="sort">自定义：</h2>
        <div class="food" v-for="(item, index) in customList" :key="index" @click="addWaitList(item)">{{ item.food }}
        </div>
      </div> -->

      <div class="content">
        <h2 class="sort">吃了啥：</h2>
        <div class="food" v-for="(item, index) in recordList" :key="index">{{
          item.value.food }}
        </div>
      </div>
    </div>

    <P style="margin-top: 2px; color: #c4c4c4;">-----😋🍲🥩🥬🥔🍄🦞🐠-----</P>
    <div class="title">计时表</div>
    <div class="from">
      <label for="food">食材:</label>
      <input type="text" placeholder="请输入食材名称" v-model="makeFood" name="makeFood" id="">
      <label for="time">时间:</label>
      <input type="text" placeholder="请输入烹饪时间" v-model="makeTime" name="makeTime" id="">
      <button type="submit" @click="submitFood(item)">开涮</button>
    </div>
    <!-- 模态框 -->
    <div class="modal" v-if="isModalVisible">
      <div class="modal-content">
        <span class="close" @click="closeModal">&times;</span>
        <p>🙀请输入食材名称和烹饪时间</p>
      </div>
    </div>

    <div class="show">{{ isShowTips ? 'Tips:锅里啥都没啦，点击食材下锅吧~' : '' }}
      <div class="show_item" v-for="(item, index) in waitList" :key="index">
        <div :style="item.value.status ? '' : 'color:rgb(87, 171, 87)'">
          <div style="font-weight: 900;">{{ index + 1 }}.{{ item.value.food }}</div>({{ item.value.nowTime }}下锅):<div
            style="display:inline-block; font-weight: 900;">{{ item.value.status ? item.value.time : '' }}</div>
          {{ item.value.status ? "秒后可吃!" : "可以吃啦~" }}
        </div>
        <button :style="item.value.status ? '' : 'color:rgb(87, 171, 87)'" @click="deleteWaitList(item)">x</button>
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
  padding: 16px 16px 0 16px;
}

.content h2 {
  color: #812228;

}

.sort {
  margin: 0 5px;
  /* padding: 5px; */
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
  margin: 0px 0 10px 0;
}

.from {
  display: flex;
  width: 100vw;
  padding: 0 20px;
  justify-content: center;
  align-items: center;
  align-content: center;
  box-sizing: border-box;
}

.from label {
  color: rgb(129, 34, 40);
  font-weight: 600;
  font-size: 14px;
}

.from input {
  width: 100px;
  padding: 0 5px;
  color: #af6f73;
}

.from input::placeholder {
  color: #af6f73;
  font-size: 12px;
}

.from button {
  width: 54px;
  height: 24px;
  background-color: #fef0f0;
  color: rgb(129, 34, 40);
  border: #af6f73 1px solid;
  border-radius: 4px;
  box-sizing: border-box;
}

.show {
  display: flex;
  flex: 1;
  justify-content: space-between;
  align-content: flex-start;
  flex-wrap: wrap;
  width: 90%;
  min-height: 300px;
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
  align-items: flex-start;
  width: 100%;
  height: auto;
}

.show_item p {
  width: 90%;
  font-size: 14px;
}

.show_item button {
  border: none;
  background-color: transparent;
  /* 去除背景颜色 */
  padding: 0;
  /* 取消内边距 */
  cursor: pointer;
  /* 可选：鼠标指针样式 */
  color: #af6f73;
  font-size: 14px;
}

/* 模态框 */
.modal {
  display: none;
  position: fixed;
  z-index: 999;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
  align-content: center;
}

/* 模态框内容 */
.modal-content {
  position: relative;
  background-color: #fefefe;
  padding: 20px;
  border: 1px solid #888;
  width: 80%;
  text-align: center;
  border-radius: 20px;
  color: #af6f73;
}

/* 关闭按钮 */
.close {
  position: absolute;
  right: 15px;
  top: 0;
  color: #af6f73;
  float: right;
  font-size: 28px;
  font-weight: bold;
}

.close:hover {
  color: black;
  cursor: pointer;
}
</style>
