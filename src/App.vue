<template>
  <div class="app" :class="{ 'bg-dark': isDark }">
    <div class="container mt-5">
      <div v-if="loading">
        <div class="progress" style="height: 10px">
          <div class="progress-bar" role="progressbar" style="width: 60%"></div>
        </div>
      </div>
      <div v-if="!loading">
        <ListingsList :listings="listings" :isDark="isDark" />
      </div>
      <button
        class="btn mt-2"
        :class="{ 'btn-light': isDark, 'btn-dark': !isDark }"
        @click="toggleDarkMode"
      >
        {{ darkModeButtonText }}
      </button>
    </div>
  </div>
</template>

<script>
import { ref, computed } from "vue";
import { useStore } from "vuex";
import ListingsList from "./components/ListingsList";

export default {
  name: "App",
  components: {
    ListingsList,
  },
  setup() {
    const store = useStore();

    const isDark = ref(false);
    const darkModeButtonText = computed(() => {
      return isDark.value ? "Helle Ansicht" : "Dunkle Ansicht";
    });
    const listings = computed(() => store.getters.listings);
    const loading = computed(() => store.getters.loading);
    const toggleDarkMode = () => {
      isDark.value = !isDark.value;
    };
    store.dispatch("getListings");

    return {
      isDark,
      darkModeButtonText,
      listings,
      loading,
      toggleDarkMode,
    };
  },
};
</script>

<style>
@import "~bootstrap/dist/css/bootstrap.min.css";

html,
body,
#app {
  width: 100%;
  height: 100%;
}
</style>

<style scoped>
.app {
  width: 100%;
  height: 100%;
}
</style>
