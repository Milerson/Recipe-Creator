<template>
  <q-layout view="lHh Lpr lFf">
    <q-header elevated>
      <q-toolbar>
        <q-toolbar-title>
          Recipe Finder
        </q-toolbar-title>
        <q-toggle
          :value="isDarkMode"
          @input="toggleDark()"
          checked-icon="check"
          color="secondary"
          label="dark mode"
          unchecked-icon="clear"
        />
      </q-toolbar>
    </q-header>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
import EssentialLink from "components/EssentialLink";
import { Dark } from "quasar";

export default {
  name: "MainLayout",

  components: {
    EssentialLink
  },

  methods: {
    toggleDark() {
      Dark.set(!Dark.isActive);
    }
  },

  computed: {
    isDarkMode() {
      return Dark.isActive;
    },
  }
};
</script>
