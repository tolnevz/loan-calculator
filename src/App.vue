<template>
  <div id="app" class="dark-bg">
    <loan-slider />
  </div>
</template>

<script>
import LoanSlider from "@/components/LoanSlider.vue";

export default {
  name: "App",
  components: {
    LoanSlider,
  },
};
</script>
<style>
.dark-gray-bg {
  background-color: #2c405a;
}
.green-bg {
  background-color: #06b038;
}
.red-bg {
  background-color: #ff4949;
}
.yellow-bg {
  background-color: #ffc82c;
}
.blue-bg {
  background-color: #78a4fa;
}

.text-white {
  color: #fff;
}
.text-warning {
  color: #ff4949;
}
.dark-bg {
  background-color: #101c2e;
}
#app {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>