<template>
  <Header />
  <div>
      <slot name="Main"></slot>
  </div>
</template>

<script>
import Header from "../components/Header.vue";

export default {
  name: "MainLayout",
  components: {
    Header,
  },
};
</script>
