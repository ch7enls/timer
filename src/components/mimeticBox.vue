<template>
    <div class="modal" v-if="isModalVisible">
        <div class="modal-content">
            <span class="close" @click="closeModal">&times;</span>
            <p>{{ modalText }}</p>
            <button @click="confirm2">确定</button>
            <button @click="cancel">取消</button>
        </div>
    </div>
</template>

<script setup>
const isModalVisible = ref(false);
const modalText = ref('')

const showModal = (text) => {
    modalText.value = text;
    isModalVisible.value = true;
};

const closeModal = () => {
    isModalVisible.value = false;
};


const confirm = () => {
    // 处理确定按钮点击事件
    console.log('确定按钮被点击');
    closeModal();
};

const confirm2 = () => {
    // 处理确定按钮点击事件
    console.log('确定按钮被点击');
    clearWaitList()
    closeModal();
};

const cancel = () => {
    // 处理取消按钮点击事件
    console.log('取消按钮被点击');
    closeModal();
};
</script>

<style lang="scss" scoped>
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

.modal-content button {
    width: 54px;
    height: 24px;
    background-color: #fef0f0;
    color: rgb(129, 34, 40);
    border: #af6f73 1px solid;
    border-radius: 4px;
    margin: 30px 10px 0 10px;
    box-sizing: border-box;
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