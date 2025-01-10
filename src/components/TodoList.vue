<template>
    <div class="todoListBox h-min min-h-full" v-if="!isMobile">
        <h2 class="py-2 ps-3 font-semibold mb-2">Demo Todo List</h2>
        <div v-for="(item, index) in items" :key="index">
            <div :class="['itemList mb-2 py-3 pl-3 cursor-pointer', { focus: item.focus }]" @click="listFocus(index)">
                {{ index + 1 }}. {{ item.title? item.title : 'Without Title' }}
            </div>
        </div>
        <div class="mx-3 mt-3 mb-[120px]">
            <button @click="createChildItem()" :class="['w-full py-2 px-3 bg-[#E7FFE9] rounded-lg hover:bg-[#81F8B1]', {maxTen: maxTen}]" :disabled="maxTen">
                {{ maxTen ? 'Disable' : 'Add Item' }}
            </button>
        </div>
        <div @click="urlImg()" class="absolute inset-x-0 bottom-0 m-5 cursor-pointer">
            <img v-if="randomImg" class="w-full h-24 rounded-lg object-cover" :src="randomImg" alt="圖片預覽" />
            <div v-else class="bg-[#EBEBEB] rounded-lg w-full h-24"></div>
        </div>
    </div>
    <div class="relative h-min min-h-full" v-if="isMobile">
        <div v-if="isSidebarOpen" class="fixed inset-0 bg-black bg-opacity-50 z-10" @click="toggleSidebar"></div>
        <div :class="['fixed top-0 h-full bg-[#A1FFC7] shadow-lg transition-all z-20 flex flex-col justify-between', isSidebarOpen ? 'left-0 w-64' : '-left-full w-16']">
            <div>
                <div class="flex items-center justify-between p-4">
                    <h2 class="font-semibold">Demo Todo List</h2>
                    <button @click="toggleSidebar" class="text-3xl font-extrabold">×</button>
                </div>
                <div v-for="(item, index) in items" :key="index">
                    <div :class="['itemList mb-2 py-3 pl-3 cursor-pointer', { focus: item.focus }]" @click="listFocus(index)">
                        {{ index + 1 }}. {{ item.title || 'Without Title' }}
                    </div>
                </div>
                <div class="m-4">
                    <button @click="createChildItem()" :class="['w-full py-2 px-3 bg-[#E7FFE9] rounded-lg hover:bg-[#81F8B1]', {maxTen: maxTen}]" :disabled="maxTen">
                        {{ maxTen ? 'Disable' : 'Add Item' }}
                    </button>
                </div>
            </div>
            <div @click="urlImg()" class="relative m-5">
                <img v-if="randomImg" class="w-full h-24 rounded-lg object-cover" :src="randomImg" alt="圖片預覽" />
                <div v-else class="bg-[#EBEBEB] rounded-lg w-full h-24"></div>
            </div>
        </div>
        <button v-if="!isSidebarOpen && isMobile" @click="toggleSidebar" class="fixed top-2 left-4 z-30 px-3 py-2 text-dark bg-white bg-opacity-75 text-lg font-black rounded-full">
            ☰
        </button>
    </div>
</template>
<script setup>
    import { ref, reactive, watch } from 'vue'
    import { defineProps, defineEmits } from 'vue'

    let isSidebarOpen = ref(false),
    toggleSidebar = () => {
        isSidebarOpen.value = !isSidebarOpen.value;
    },
    isMobile = ref(window.innerWidth <= 768);

    window.addEventListener('resize', () => {
        isMobile.value = window.innerWidth <= 768;
    });

    const emit = defineEmits(['item', 'itemAll', 'randomImg']),
        props = defineProps({
            focusItem: Object,
            parentItems: Array,
        });

    let items = reactive([...props.parentItems]),
    maxTen = ref(false),
    randomImg = ref(''),
    urlImg = () => {
        randomImg.value = `https://picsum.photos/200?random=${Math.random()}`;
        emit('randomImg', randomImg.value);
    };

    const listFocus = (index) => {
        items.forEach((item) => (item.focus = false));
        items[index].focus = true;
        emit('item', items[index]);
    };

    const createChildItem = ()=> {
        if (!props.focusItem.url) {
            props.focusItem.url = randomImg.value;
        }
        const newItem = { ...props.focusItem };
        items.forEach((item) => (item.focus = false));
        items.push({
            ...newItem,
            focus: true,
            id: items.length + 1,
        });
        emit('itemAll', items);
    }

    emit('itemAll', items);

    watch(() => props.parentItems, (newParentItems) => {
            items.splice(0, items.length, ...newParentItems);
            maxTen.value = items.length === 10 ? true : false;
        },
        { deep: true }
    );
</script>
<style scoped>
    .todoListBox {
        padding: 10px 0px;
        width: 250px;
        background-color: #A1FFC7;
        position: relative;
    }

    .itemList {
        font-weight: normal;
        position: relative;
        background-color: #81F8B1;
    }

    .itemList.focus {
        font-weight: 900;
    }

    .itemList.focus::after {
        content: '';
        position: absolute;
        top: 50%;
        right: -9.8px;
        transform: translateY(-50%) rotate(90deg);
        width: 0;
        height: 0;
        border-left: 25px solid transparent;
        border-right: 24px solid transparent;
        border-top: 30px solid white;
    }

    .maxTen {
        background-color: #E7FFE9;
        color: #EBEBEB;
    }
</style>
