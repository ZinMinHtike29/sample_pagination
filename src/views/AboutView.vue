<template>
  <div>
    <h1>{{ testHtml }}</h1>
    <span v-html="testHtml"></span>
    <input type="text" v-model="testMsg" placeholder="Test Data" />
  </div>
</template>

<script>
import debounce from "lodash.debounce";
export default {
  data() {
    return {
      testHtml: "<h1 class='text-red-500'>About Page</h1>",
      testMsg: "",
    };
  },
  watch: {
    testMsg(newValue, oldValue) {
      this.debouncedFetch(newValue, oldValue);
    },
  },

  mounted() {
    this.debouncedFetch = debounce((newValue, oldValue) => {
      console.log("value changed: ", newValue);
      // call fetch API to get results
    }, 1000);
  },
};
</script>

<style lang="stylus" scoped></style>

