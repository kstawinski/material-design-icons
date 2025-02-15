<template>
  <v-app-bar app color="primary" dark flat>
    <v-toolbar-title>{{ title }}</v-toolbar-title>
    <v-spacer></v-spacer>

    <v-icon class="mr-1" style="opacity: .8;">mdi-theme-light-dark</v-icon>
    <v-tooltip bottom>
      <template v-slot:activator="{ on, attrs }">
        <div
          class="d-flex mr-2"
          v-bind="attrs"
          v-on="on"
        >
          <v-checkbox
            v-model="$vuetify.theme.dark"
            @click="setDarkMode"
            color="white"
            hide-details
          ></v-checkbox>
        </div>
      </template>
      <span>{{ $vuetify.theme.dark ? 'Disable' : 'Enable' }} dark mode</span>
    </v-tooltip>

    <v-menu transition="slide-y-transition" nudge-bottom="20" :close-on-content-click="true">
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          icon
          v-bind="attrs"
          v-on="on"
        >
          <v-icon>mdi-dots-vertical</v-icon>
        </v-btn>
      </template>
      <v-list dense style="padding: 3px;">
        <v-list-item
          v-for="(button, i) in buttons"
          :key="i"
          :link="true"
          @click="emitClick(button.emitName)"
        >
          <v-list-item-icon>
            <v-icon>{{ button.icon }}</v-icon>
          </v-list-item-icon>
          <v-list-item-content>
            <v-list-item-title>{{ button.name }}</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-menu>
  </v-app-bar>
</template>

<script lang="ts">
import Vue from 'vue';
import eventBus from '../plugins/eventBus';

export default Vue.extend({
  name: 'Header',

  data: () => ({
    title: 'Material Design Icons',
    buttons: [
      { name: 'Random icon', icon: 'mdi-shuffle', emitName: 'random' },
      { name: 'About', icon: 'mdi-information-outline', emitName: 'about' },
    ],
  }),

  methods: {
    setDarkMode() {
      localStorage.setItem('darkmode', this.$vuetify.theme.dark.toString());
    },
    // Emit click on some menu item with emitName property included in @click event
    emitClick(name: string) {
      eventBus.$emit(name);
    },
  },
});
</script>
