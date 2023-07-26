<template>
    <div class="container">
        <div v-for="(item, index) in list" :key="index" class="box" :style="{ backgroundColor: item.background }">
            {{ index }}
        </div>
        <div v-if="loading" class="loading">Loading...</div>
    </div>
</template>

<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue';
const list = ref([{ background: "rgb(233, 32, 38)" }]);
const loading = ref(false);
onMounted(() => {
    loadMoreItems();
    window.addEventListener('scroll', handleScroll);
});

onUnmounted(() => {
    window.removeEventListener('scroll', handleScroll);
});


const getRandomColor = () => {
    return `rgb(${Math.floor(Math.random() * 256)}, ${Math.floor(Math.random() * 256)}, ${Math.floor(Math.random() * 256)})`;
}


const loadMoreItems = async () => {
    if (loading.value) return;
    loading.value = true;
    await new Promise(resolve => setTimeout(resolve, Math.random() * 5000));
    const newData = getList(10);
    console.log('newData==>', newData)
    list.value.push(...newData);
    loading.value = false;
}


const getList = (num: number) => {
    return Array.from({ length: num }, () => ({ background: getRandomColor() }));
}


const handleScroll = () => {
    const scrollY = window.scrollY;
    const windowHeight = window.innerHeight;
    const documentHeight = document.documentElement.scrollHeight;
    const threshold = documentHeight - windowHeight - 500;
    if (scrollY > threshold && list.value.length < 50) {
        loadMoreItems();
    }
}
</script>

<style scoped>
.container {
    display: flex;
    flex-direction: column;
    margin-left: -312px;
    width: 100vw;
}


.box {
    width: 100%;
    height: 500px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 20px;
    font-weight: bold;
    color: white;
}

.loading {
    width: 100%;
    height: 50px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 18px;
    font-weight: bold;
    color: #333;
    background-color: #f0f0f0;
}
</style>
