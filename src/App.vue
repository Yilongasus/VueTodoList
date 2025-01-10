<template>
    <div class="flex h-full">
        <div class="flex-2">
            <TodoList @item="updateChildItem" @itemAll="updateItems" @randomImg="randomImg" :focusItem="childItem" :parentItems="items" class="h-full py-5" />
        </div>
        <div class="flex-1 h-screen m-6 mx-7 flex flex-col">
            <div class="flex w-full mt-0 sm:mt-6 text-right flex flex-row-reverse">
                <div><img class="w-5 h-5" src="/src/assets/delete.jpg" @click="deleteChildItem(childItem.id)" alt="刪除"></div>
            </div>
            <div class="flex flex-col sm:flex-row w-full">
                <div class="title flex-col sm:flex-row mr-5 w-full sm:w-1/2 mb-2">
                    <div><label for="title" class="">Title</label></div>
                    <div><input type="text" name="title" id="title" class="w-full" v-model="childItem.title"/></div>
                </div>
                <div class="Date flex-col sm:flex-row w-1/2 w-full">
                    <div><label for="startDate" class="">Date</label></div>
                    <div class="flex items-center w-full">
                        <div class="w-full"><input type="date" name="startDate" id="startDate" class="w-full" @change="startDateChange" v-model="childItem.startDate"/></div>
                        <div class="mx-2">~</div>
                        <div class="w-full"><input type="date" name="endDate" id="endDate" class="w-full" @change="endDateChange"  v-model="childItem.endDate"/></div>
                    </div>
                </div>
            </div>
            <div class="flex flex-col sm:flex-row w-full mt-3 items-stretch max-h-[300px] sm:max-h-[151px]">
                <div class="image flex flex-col mr-5 w-full sm:w-1/2 justify-between">
                    <div><label for="Image" class="w-full">Image</label></div>
                    <div><button class="px-6 py-3 w-full" @click="uploadImg()">Upload Image</button></div>
                    <div class="text-center py-2">or</div>
                    <div class="w-full">
                        <input type="text" id="imageUrl" class="w-full" name="imageUrl" placeholder="請輸入圖片網址" v-model="childItem.url" />
                    </div>
                </div>
                <div class="flex flex-col sm:flex-row items-center justify-center mt-3 sm:mt-3 w-full sm:w-1/2 bg-[#EBEBEB] rounded-lg overflow-hidden">
                    <img v-if="childItem.url" class="w-full h-full rounded-lg object-cover" :src="childItem.url" alt="圖片預覽" />
                </div>
            </div>
            <div class="flex flex-col sm:flex-row formRow mt-3 w-full">
                <div><label for="content" class="">Content</label></div>
                <div class="relative">
                    <textarea class="w-full p-0 pl-[0.5rem] pt-2" rows="7" name="content" id="content" maxlength="200" v-model="childItem.content"></textarea>
                    <div class="wordCount absolute bottom-1.5 right-0 py-3">200/{{ childItem.content.length }}</div>
                </div>
            </div>
        </div>
    </div>
</template>
<script setup>
    import TodoList from './components/TodoList.vue'
    import { ref, reactive, watch } from 'vue'
    import Swal from 'sweetalert2'

    let items = reactive([]),
        childItem = reactive({
            id: 1,
            title: '',
            startDate: '',
            endDate: '',
            url: '',
            content: '',
        })
    
    let randomImg = (img) => {
        childItem.url = img;
    }

    let uploadImg = () => {
        const input = document.createElement('input');
        input.type = 'file';
        input.accept = 'image/*';
        input.onchange = (event) => {
            const file = event.target.files[0];
            if (file) {
                const reader = new FileReader();
                reader.onload = (e) => {
                    childItem.url = e.target.result;
                };
                reader.readAsDataURL(file);
            }
        };
        input.click();
    };

    // 所有
    const updateItems = (newItems) => {
        items.splice(0, items.length, ...newItems);
    };

    // 被選取的
    const updateChildItem = (item) => {
        Object.assign(childItem, item);
    };

    const deleteChildItem = (id) => {
        const index = items.findIndex((item) => item.id === id);
        if (index !== -1) {
            items.splice(index, 1);
        }
    };

    watch(() => childItem, (value) => {
        Object.assign(childItem, value)
    })

    watch(() => items, (value) => {
        Object.assign(items, value)
    })

    let startDateChange = ()=> {
        if (childItem.startDate && childItem.endDate) {
            if (new Date(childItem.startDate) > new Date(childItem.endDate)) {
                Swal.fire({
                    title: "起始日期不得大於結束日期",
                    icon: "error",
                    confirmButtonText: '了解',
                    showCloseButton: true,
                });
                childItem.startDate = '';
                return;
            }
            else if (new Date(childItem.startDate).getTime() === new Date(childItem.endDate).getTime()) {
                Swal.fire({
                    title: "日期不可重複",
                    icon: "error",
                    confirmButtonText: '了解',
                    showCloseButton: true,
                });
                childItem.startDate = '';
            }
        }
    }

    let endDateChange = ()=> {
        if (childItem.startDate && childItem.endDate) {
            if (new Date(childItem.startDate).getTime() > new Date(childItem.endDate).getTime()) {
                Swal.fire({
                    title: "結束日期不得小於起始日期",
                    icon: "error",
                    confirmButtonText: '了解',
                    showCloseButton: true,
                });
                childItem.endDate = '';
            }
            else if (new Date(childItem.startDate).getTime() === new Date(childItem.endDate).getTime()) {
                Swal.fire({
                    title: "日期不可重複",
                    icon: "error",
                    confirmButtonText: '了解',
                    showCloseButton: true,
                });
                childItem.endDate = '';
            }
        }
    }
</script>
<style scoped>
    html,
    body {
        color: #000000;
        padding: 0;
        margin: 0;
    }

    #app {
        font-family: "Microsoft Jhenghei", "Avant Garde", sans;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        position: relative;
        width: 100%;
        height: 100%;
    }

    input {
        background-color: #EBEBEB;
        border-radius: 5px;
        outline: none;
        padding: 0.5rem 0.75rem;
    }

    textarea {
        background-color: #EBEBEB;
        border-radius: 5px;
        outline: none;
    }

    button {
        background-color: #E7FFE9;
    }

    button:disabled {
        color: #EBEBEB;
    }

    button:hover {
        background-color: #81F8B1;
    }

    .formRow {
        display: flex;
        flex-direction: column;
    }

    .imagePreview img {
        max-width: 200px;
        border: 1px solid #ccc;
    }

    .wordCount {
        display: flex;
        align-items: center;
        justify-content: center;
        width: 80px;
        height: 35px;
        background-color: #E7FFE9;
        border-radius: 5px;
        font-size: 12px;
        color: black;
    }

    .wordCount::before {
        content: '';
        position: absolute;
        top: 50%;
        left: -30px;
        transform: translateY(-50%) rotate(90deg);
        width: 0;
        height: 0;
        border-left:35px solid transparent;
        border-top: 30px solid #E7FFE9;
    }
</style>