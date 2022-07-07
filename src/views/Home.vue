<template>
  <MainLayout> 
      <template v-slot:Main>
          <ListUser/>
      </template>
  </MainLayout>
</template>

<script>
import MainLayout from "../layout/MainLayout.vue";
import ListUser from "../components/ListUser.vue";

export default {
  name: "Home",
  components: {
    MainLayout,
    ListUser,
  },
};
</script>
