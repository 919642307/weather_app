<template>
    <div class="flex flex-col min-h-screen font-Roboto bg-weather-primary">
        <SiteNavigation></SiteNavigation>
        <!-- <HomeView /> -->
        <RouterView />
    </div>
</template>

<script setup>
import { RouterView } from "vue-router";
import SiteNavigation from "./components/SiteNavigation.vue";
import BaseModal from "./components/BaseModal.vue";
import HomeView from "./views/HomeView.vue";
</script>

<style lang="scss" scoped></style>
