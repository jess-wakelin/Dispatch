<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <!-- <HelloWorld msg="Welcome to Your Vue.js App" /> -->
    <TabSystem :default-title="`New Request`">
      <APIPanel></APIPanel>
    </TabSystem>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from "@/components/HelloWorld.vue";
import TabSystem from "@/components/TabSystem.vue";
import APIPanel from "@/components/APIPanel.vue";

export default {
  name: "Home",
  components: {
    // HelloWorld,
    TabSystem,
    APIPanel,
  },
};
</script>

// put postman on the tab panel
