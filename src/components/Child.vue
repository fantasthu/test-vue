<template>
  <div class="child">{{ message }}</div>
</template>

<script>
export default {
  name: "Child",
  props: { message: "", childData: null },
  data() {
    return {};
  },
  watch: {
    message(val) {
      console.log("watch-child-val :", val);
    },
    childData: {
      handler(val) {
        console.log("child-data :", val);
      },
      immediate: true
    }
  },
  beforeCreate() {
    console.log("child-beforeCreated :", "beforeCreated");
  },
  created() {
    console.log("child-created :", "created");
  },
  mounted() {
    console.log("child-mounted :", "mounted");
  },
  beforeUpdate() {
    console.log("child-beforeUpdate :", "beforeUpdate");
  },
  updated() {
    console.log("child-updated :", "updated");
  },
  beforeDestroy() {
    console.log("beforeDestroy :", "beforeDestroy");
  },
  destroyed() {
    console.log("destroyed :", "destroyed");
  }
};
</script>

<style>
</style>