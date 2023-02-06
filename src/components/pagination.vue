<template>
  <div>
    <div class="flex justify-end">
      <div class="flex gap-3" v-if="pagLink.length > 0">
        <button
          v-if="pagLink[0].url != null"
          class="
            py-1.5
            px-3
            border-0
            bg-transparent
            outline-none
            transition-all
            duration-300
            rounded-full
            focus:shadow-none
            block
            md:hidden
          "
          @click="changePage(pagLink[0])"
        >
          {{ pagLink[0].label }}
        </button>
        <button
          v-if="pagLink[pagLink.length - 1].url != null"
          class="
            py-1.5
            px-3
            border-0
            bg-transparent
            outline-none
            transition-all
            duration-300
            rounded-full
            focus:shadow-none
            block
            md:hidden
          "
          @click="changePage(pagLink[pagLink.length - 1])"
        >
          {{ pagLink[pagLink.length - 1].label }}
        </button>
      </div>
      <nav aria-label="Page navigation example">
        
        <ul class="list-style-none hidden md:flex">
          <li
            class="page-item"
            v-for="(l, i) in pagLink"
            :key="i"
            @click="changePage(l)"
          >
            <a
              v-if="l.url != null"
              class="
                page-link
                block
                py-1.5
                px-3
                border-0
                bg-transparent
                outline-none
                transition-all
                duration-300
                rounded-full
                pointer-events-none
                focus:shadow-none
              "
              href="#"
              tabindex="-1"
              :class="[l.active ? 'bg-blue-600 text-white' : 'text-black']"
              >{{ l.label }}</a
            >
          </li>
        </ul>
      </nav>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Pagination",
  props: {
    links: {},
  },
  data() {
    return {
      currentPage: 1,
    };
  },
  watch: {
    pagLink(newValue, oldValue) {
      this.pagLink.filter((item) => {
        if (item.active) {
          this.currentPage = item.label;
        }
      });
    },
  },
  computed: {
    pagLink() {
      if (this.links.length > 0) {
        this.links[0].label = "Previous";
        this.links[this.links.length - 1].label = "Next";
      }
      return this.links;
    },
  },
  methods: {
    changePage(v) {
      let page;
      switch (v.label) {
        case "Next":
          page = this.currentPage * 1 + 1;
          break;
        case "Previous":
          page = this.currentPage * 1 - 1;
          break;
        default:
          page = v.label;
          break;
      }
      let url = `http://localhost/TestServer/public/api/searchResult?searchKey=${this.$route.params.searchKey}&page=${page}`;
      axios.get(url).then((res) => {
        this.$emit("data", res.data);
      });
    },
  },
  mounted() {},
};
</script>

<style lang="stylus" scoped></style>