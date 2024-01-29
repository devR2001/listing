<template>
  <div id="listings">
    <Notification
      :notification="notification"
      :toggleNotification="toggleNotification"
    />
    <div class="row row-cols-1 row-cols-md-3 g-4">
      <div class="col" v-for="listing in listings" :key="listing.id">
        <ListingsListItem :listing="listing" />
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
import { onMounted } from "vue";
import ListingsListItem from "./ListingsListItem";
import Notification from "./Notification";
import useNotification from "@/hooks/useNotification";
import useDarkMode from "@/hooks/useDarkMode";
import { useStore } from "vuex";

export default {
  name: "ListingsList",
  components: {
    ListingsListItem,
    Notification,
  },
  props: ["listings"],
  setup() {
    const store = useStore();
    const { notification, setNotification, toggleNotification } =
      useNotification();
    const { darkMode } = useDarkMode();
    // const notification = ref(null);

    const resetListings = () => {
      setNotification("Liste wurde zurückgesetzt.");
      store.dispatch("resetListings");
    };

    onMounted(() => {
      setNotification("Herzlich Willkommen!");
      // notification.value = "Herzlichen Willkommen!";

      // setTimeout(() => {
      //   notification.value = null;
      // }, 3000);
    });

    return {
      darkMode,
      notification,
      resetListings,
      toggleNotification,
    };
  },
};
</script>
