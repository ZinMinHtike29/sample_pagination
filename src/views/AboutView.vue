<template>
  <div>
    <h1>{{ TestServer }}</h1>
    <span :v-html="testHtml"></span>
  </div>
</template>

<script>
export default {
  data() {
    return {
      testHtml: "<h1 class='text-red-500'>About Page</h1>",
    };
  },
};
</script>

<style lang="stylus" scoped></style>

