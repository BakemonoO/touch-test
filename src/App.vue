<template>
  <div class="app">
    <text-editor />
  </div>
</template>

<script>
import TextEditor from "./components/TextEditor.vue";
export default {
  components: { TextEditor }
};
</script>

<style scoped>
.app {
  padding: 20px;
}
</style>
