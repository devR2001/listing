<template>
  <div id="listings">
    <Notification :notification="notification" :isDark="isDark" />
    <div class="row row-cols-1 row-cols-md-3 g-4">
      <div class="col" v-for="listing in listings" :key="listing.id">
        <ListingsListItem :listing="listing" :isDark="isDark" />
      </div>
    </div>
    <button
      class="btn btn-primary mt-2"
      @click="resetListings"
      :disabled="listings.length === 3"
    >
      Reset
    </button>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";
import ListingsListItem from "./ListingsListItem";
import Notification from "./Notification";
import { useStore } from "vuex";

export default {
  name: "ListingsList",
  components: {
    ListingsListItem,
    Notification,
  },
  props: ["listings", "isDark"],
  setup() {
    const store = useStore();
    const notification = ref(null);

    const resetListings = () => store.dispatch("resetListings");

    onMounted(() => {
      notification.value = "Herzlichen Willkommen!";

      setTimeout(() => {
        notification.value = null;
      }, 3000);
    });
    return {
      notification,
      resetListings,
    };
  },
};
</script>
