<template>
  <el-dialog title="提示" :visible.sync="value" width="30%" :before-close="close">
    <span>这是一段信息</span>
    <span slot="footer" class="dialog-footer">
      <el-button @click="close">取 消</el-button>
      <el-button type="primary" @click="handleSubmit">确 定</el-button>
    </span>
  </el-dialog>
</template>

<script>
export default {
  name: "ChildModel",
  props: {
    value: {
      type: Boolean,
      default: false
    }
  },
  data() {
    return {};
  },
  watch: {},
  computed: {},
  methods: {
    close() {
      this.$emit("input", false);
    },
    handleSubmit() {}
  }
};
</script>

<style>
</style>