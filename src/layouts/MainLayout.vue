<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        />

        <q-toolbar-title> Sion Band </q-toolbar-title>

        <div></div>
      </q-toolbar>
    </q-header>

    <q-drawer v-model="leftDrawerOpen" show-if-above bordered>
      <q-list v-if="isAdmin">
        <q-item-label header> </q-item-label>

        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
          :link="link"
          :handleClick="logout"
        />
      </q-list>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script setup>
import { computed, onMounted, ref } from "vue";
import { useRouter } from "vue-router";
import { storeToRefs } from "pinia";
import { supabase } from "../boot/supabase";
import linksList from "../sources/links";
import { useAuthStore } from "../store/auth";
import EssentialLink from "components/EssentialLink.vue";

const authStore = useAuthStore();
const { isAdmin } = storeToRefs(authStore);
onMounted(async () => {
  await authStore.loadUser();
});

const leftDrawerOpen = ref(false);

const essentialLinks = linksList;

const { user } = storeToRefs(authStore);
const logout = async () => {
  await supabase.auth.signOut();
  router.push({ path: "/login" });
};

const toggleLeftDrawer = () => {
  leftDrawerOpen.value = !leftDrawerOpen.value;
};

const router = useRouter();
</script>
