<template>
  <div class="mt-3">
    <div class="mb-3 w-2/4 m-auto">
      <div class="relative">
        <input
          type="text"
          class="
            w-full
            form-control
            block
            pl-3
            pr-10
            py-1.5
            text-base
            font-normal
            text-gray-700
            bg-white bg-clip-padding
            border border-solid border-gray-300
            rounded
            transition
            ease-in-out
            m-0
            focus:text-gray-700
            focus:bg-white
            focus:border-blue-600
            focus:outline-none
          "
          v-model="searchKey"
          @keypress="detactInput"
          @keyup="searchData"
          id="exampleFormControlInput1"
          placeholder="Search User"
        />
        <i
          class="fa-solid fa-xmark absolute right-2 top-[30%] text-red-500"
          v-if="searchStatus"
          @click="hideBox"
        ></i>
      </div>
      <div
        class="mt-1 bg-slate-100 py-2 px-2 rounded-md shadow-sm"
        v-if="searchStatus"
      >
        <div class="">
          <div class="flex justify-center" v-if="fetchStatus">
            <div
              class="
                spinner-border
                animate-spin
                inline-block
                w-4
                h-4
                border-1
                text-teal-400
                rounded-full
              "
              role="status"
            >
              <span class="visually-hidden">Loading...</span>
            </div>
          </div>
          <div class="" v-if="!fetchStatus">
            <div class="font-bold" v-if="userList.length == 0">
              There Is No User With This Name.
            </div>
            <div class="" v-if="userList.length > 0">
              <div class="border-b py-2" v-for="u in userList" :key="u">
                {{ u.name }}
              </div>
              <div class="text-teal-700 mt-2 text-end" @click="toResultPage">
                viewall <i class="fa-solid fa-right-long ml-1"></i>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "HomeView",
  data() {
    return {
      searchKey: "",
      searchStatus: false,
      userList: "",
      fetchStatus: false,
      timer: null,
    };
  },
  methods: {
    detactInput() {
      clearTimeout(this.timer);
      console.log("detacting");
    },
    searchData() {
      clearTimeout(this.timer);
      this.timer = setTimeout(() => {
        this.searchStatus = true;
        this.fetchStatus = true;
        axios
          .get("http://localhost/TestServer/public/api/searchUser", {
            params: { searchKey: this.searchKey },
          })
          .then((res) => {
            this.fetchStatus = false;
            console.log(res.data.searchUser);
            this.userList = res.data.searchUser;
            // // console.log(this.searchList);
          });
      }, 1000);
    },
    toResultPage() {
      this.$router.push(`result/searchKey=${this.searchKey}`);
    },
    hideBox() {
      this.searchStatus = false;
      this.searchKey = "";
    },
  },
};
</script>

<style lang="stylus" scoped></style>

