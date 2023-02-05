<template>
  <div>
    <div class="flex flex-col w-[80%] m-auto">
      <div
        class="overflow-x-auto"
        :style="currentHeight < maxHeight ? `height:${maxHeight + 66}px` : ''"
      >
        <div class="py-2 inline-block min-w-full">
          <div class="overflow-x-auto">
            <table class="min-w-full">
              <thead class="border-b">
                <tr>
                  <th
                    scope="col"
                    class="
                      text-sm
                      font-medium
                      text-gray-900
                      px-6
                      py-4
                      text-left
                    "
                  >
                    Id
                  </th>
                  <th
                    scope="col"
                    class="
                      text-sm
                      font-medium
                      text-gray-900
                      px-6
                      py-4
                      text-left
                    "
                  >
                    Name
                  </th>
                  <th
                    scope="col"
                    class="
                      text-sm
                      font-medium
                      text-gray-900
                      px-6
                      py-4
                      text-left
                    "
                  >
                    Email
                  </th>
                  <th
                    scope="col"
                    class="
                      text-sm
                      font-medium
                      text-gray-900
                      px-6
                      py-4
                      text-left
                    "
                  >
                    Created_at
                  </th>
                </tr>
              </thead>
              <tbody id="tableHeight">
                <tr class="border-b" v-for="u in userList" :key="u">
                  <td
                    class="
                      px-6
                      py-4
                      whitespace-nowrap
                      text-sm
                      font-medium
                      text-gray-900
                    "
                  >
                    {{ u.id }}
                  </td>
                  <td
                    class="
                      text-sm text-gray-900
                      font-light
                      px-6
                      py-4
                      whitespace-nowrap
                    "
                  >
                    {{ u.name }}
                  </td>
                  <td
                    class="
                      text-sm text-gray-900
                      font-light
                      px-6
                      py-4
                      whitespace-nowrap
                    "
                  >
                    {{ u.email }}
                  </td>
                  <td
                    class="
                      text-sm text-gray-900
                      font-light
                      px-6
                      py-4
                      whitespace-nowrap
                    "
                  >
                    {{ formatDate(u.created_at) }}
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
      <div class="">
        <Pagination :links="pagLink" @data="getEmitData"></Pagination>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Pagination from "@/components/pagination.vue";
export default {
  name: "ResultPage",
  components: {
    Pagination,
  },
  data() {
    return {
      fetchStatus: false,
      userList: "",
      maxHeight: "",
      currentHeight: "",
      pagLink: "",
    };
  },
  methods: {
    getSearchResult() {
      this.fetchStatus = true;
      axios
        .get("http://localhost/TestServer/public/api/searchResult", {
          params: { searchKey: this.$route.params.searchKey },
        })
        .then((res) => {
          this.fetchStatus = false;
          this.userList = res.data.searchUser.data;
          this.pagLink = res.data.searchUser.links;
          let tbodyTag = document.querySelector("#tableHeight");
          setTimeout(() => {
            this.maxHeight = tbodyTag.offsetHeight;
            this.currentHeight = tbodyTag.offsetHeight;
          }, 1);
          // // console.log(this.searchList);
        });
    },
    formatDate(date) {
      let changeDate = new Date(date);
      var options = { year: "numeric", month: "short", day: "numeric" };
      changeDate = changeDate.toLocaleDateString("en-Us", options);
      return changeDate;
    },
    getEmitData(e) {
      this.userList = e.searchUser.data;
      this.pagLink = e.searchUser.links;
      let tbodyTag = document.querySelector("#tableHeight");
      this.currentHeight = tbodyTag.offsetHeight;
      setTimeout(() => {
        this.currentHeight = tbodyTag.offsetHeight;
      }, 1);
    },
  },
  mounted() {
    this.getSearchResult();
  },
};
</script>

<style lang="stylus" scoped></style>
