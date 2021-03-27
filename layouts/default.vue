<template>
  <v-app dark style="background-color:#1976d2  overflow-x: hidden;">
    <v-app-bar
      absolute
      color="#1976d2"
      dark
      shrink-on-scroll
      prominent
      fade-img-on-scroll
      scroll-target="#scrolling-techniques-3"
    >
      <v-icon left large class="pt-6">mdi-apps</v-icon>
      <span class="pt-6">KLEDO PAYMENT</span>

      <v-spacer></v-spacer>
    </v-app-bar>
    <v-main
      class="overflow-y-auto"
      style="overflow-x: hidden; background-color: #fce9e5"
    >
      <div style="height: 120px"></div>

      <nuxt />
    </v-main>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: "mdi-apps",
          title: "Welcome",
          to: "/",
        },
        {
          icon: "mdi-chart-bubble",
          title: "Inspire",
          to: "/inspire",
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: "Kledo Payment",
    };
  },
};
</script>
