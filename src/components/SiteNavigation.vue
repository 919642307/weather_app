<template>
    <header class="sticky top-0 bg-weather-primary shadow-lg">
        <nav
            class="container flex flex-col sm:flex-row itmes-center gap-4 text-white py-6"
        >
            <RouterLink :to="{ name: 'home' }">
                <div class="flex items-center gap-3">
                    <i class="fa-solid fa-sun text-2xl"></i>
                    <p class="text-2xl">天气</p>
                </div>
            </RouterLink>
            <div class="flex gap-3 flex-1 justify-end">
                <i
                    @click="toggleModal"
                    class="fa-solid fa-circle-info text-xl hover:text-weather-secondary duration-150 cursor-pointer"
                ></i>
                <i
                    class="fa-solid fa-plus text-xl hover:text-weather-secondary duration-150 cursor-pointer"
                    @click="addCity"
                    v-if="route.query.preview"
                ></i>
            </div>
            <BaseModal
                :modalActive="modalActive"
                @close-modal="toggleModal"
                class="text-black"
            >
                <h1>
                    <div class="text-black">
                        <h1 class="text-2xl mb-1">这是什么:</h1>
                        <p class="mb-4">
                            “天气”允许您查看您选择的城市当前的天气以及的未来天气。
                        </p>
                        <h2 class="text-2xl">帮助:</h2>
                        <ol class="list-decimal list-inside mb-4">
                            <li>通过在搜索栏中输入名称来搜索您的城市。</li>
                            <li>在结果中选择一个城市。</li>
                            <li>
                                通过单击“+”右上角的图标， 这将保存城市以便查看。
                            </li>
                        </ol>

                        <h2 class="text-2xl">移除城市</h2>
                        <p>
                            如果您不再希望跟踪城市，只需选择
                            主页中的城市。在底部页面上，将有删除城市的选项。
                        </p>
                    </div>
                </h1>
            </BaseModal>
        </nav>
    </header>
</template>

<script setup>
import { ref } from "vue";
import { RouterLink, useRoute, useRouter } from "vue-router";
import { uid } from "uid";
import BaseModal from "./BaseModal.vue";
const savedCities = ref([]);
const route = useRoute();
const router = useRouter();
const addCity = () => {
    if (localStorage.getItem("saveCities")) {
        savedCities.value = JSON.parse(localStorage.getItem("savedCities"));
    }

    const locationObj = {
        id: uid(),
        state: route.params.state,
        city: route.params.city,
        coords: {
            lat: route.query.lat,
            lng: route.query.lng,
        },
    };
    savedCities.value.push(locationObj);
    localStorage.setItem("savedCities", JSON.stringify(savedCities.value));

    let query = Object.assign({}, route.query);
    delete query.preview;
    query.id = locationObj.id;
    router.replace({ query });
};
const modalActive = ref(null);
const toggleModal = () => {
    modalActive.value = !modalActive.value;
};
</script>
