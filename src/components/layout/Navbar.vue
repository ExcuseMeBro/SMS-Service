<template>
  <div class="bg-zinc-100 h-20 py-4 sticky-top flex justify-between items-center">
    <div class="ml-5">
      <div class="relative">
        <span class="absolute inset-y-0 left-0 flex pl-2 items-center">
          <button type="submit" class="p-4 focus:outline-none focus:shadow-outline">
            <svg fill="none" stroke="currentColor" stroke-linecap="round" stroke-linejoin="round" stroke-width="2" viewBox="0 0 24 24" class="text-gray-500 w-6 h-6"><path d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
          </button>
        </span>
        <input type="search" name="search" class="bg-gray-200 text-gray-500 py-3 text-lg w-96 rounded-md pl-16 focus:outline-none focus:bg-gray-200 outline-none border-0" placeholder="Search..." autocomplete="off" />
      </div>
    </div>
    <div class="mr-5 flex items-center">
      <button class="inline-block relative bg-white text-gray-500 p-2 mr-3 rounded-full hover:bg-gray-200 shadow">
        <svg class="px-1 cursor-pointer" width="27" height="27" viewBox="0 0 20 22" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path fill-rule="evenodd" clip-rule="evenodd" d="M10 19C15.3739 19 19 17.9032 19 15.1613C19 12.4194 18 12.4194 17.2 11.871C17.2 8.59908 16 2 10 2C4 2 2.8 8.59908 2.8 11.871C2 12.4194 1 13.1419 1 15.1613C1 18.4516 4.6271 19 10 19Z" stroke="#222628" stroke-width="1.4" stroke-linecap="round" stroke-linejoin="round" />
          <path d="M13 19C13 19 12.5 21 10 21C7.5 21 7 19 7 19" stroke="#222628" stroke-width="1.4" stroke-linecap="round" stroke-linejoin="round" />
          <path d="M10 1V2" stroke="#222628" stroke-width="1.4" stroke-linecap="round" />
        </svg>
        <div class="absolute top-0.5 right-0 p-1.5 bg-rose-500 rounded-full"></div>
      </button>
      <div class="border-l-2 pl-3 flex items-center">
        <div class="inline-block relative bg-white text-gray-500 p-2 rounded-full hover:bg-gray-200 shadow">
          <svg class="px-1" width="27" height="27" viewBox="0 0 18 20" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path
              d="M1.40334 15.3222C1.44 14.9879 1.45832 14.8208 1.48127 14.7015C1.75983 13.2543 2.93043 12.2799 4.40422 12.2686C4.52566 12.2677 4.71526 12.2816 5.09447 12.3095C6.27227 12.3959 7.599 12.4667 8.78541 12.4667C10.0237 12.4667 11.3969 12.3896 12.5907 12.298C12.9564 12.27 13.1392 12.256 13.2581 12.2563C14.6545 12.2601 15.7825 13.1264 16.1463 14.4745C16.1773 14.5893 16.2069 14.748 16.2661 15.0653V15.0653C16.3314 15.4153 16.364 15.5902 16.3767 15.7324C16.524 17.3759 15.3162 18.8298 13.6735 18.9865C13.5314 19 13.3534 19 12.9975 19L8.78541 19H4.69989C4.37599 19 4.21405 19 4.08163 18.9882C2.50997 18.8487 1.31503 17.5155 1.34763 15.938C1.35038 15.8051 1.36803 15.6441 1.40334 15.3222V15.3222Z"
              stroke="#222628"
              stroke-width="1.4"
            />
            <path d="M5 5C5 2.79086 6.79086 1 9 1V1C11.2091 1 13 2.79086 13 5V5.57143C13 7.46498 11.465 9 9.57143 9V9H8.42857V9C6.53502 9 5 7.46498 5 5.57143V5Z" stroke="#222628" stroke-width="1.4" />
          </svg>
        </div>
        <button id="dropdownBtn" @click="toggleDropDown()" class="px-4 py-2.5 text-center inline-flex items-center" type="button">
          <strong class="text-lg font-semibold px-2 cursor-pointer text-zinc-900 hover:text-rose-500 capitalize">{{ name }}</strong>
          <svg class="w-4 h-4 ml-2" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 9l-7 7-7-7"></path></svg>
        </button>
        <div id="dropdownInformation" class="z-10 hidden bg-white divide-y divide-gray-100 rounded shadow w-44">
          <ul class="py-1 text-sm text-gray-700 dark:text-gray-200" aria-labelledby="dropdownInformationButton">
            <li>
              <a href="#" class="block px-4 py-2 hover:bg-gray-100"><i class="fa fa-user"></i> Profil</a>
            </li>
          </ul>
          <div class="py-1">
            <a href="/login" @click="onLogout()" class="block px-4 py-2 text-gray-700 hover:bg-gray-100"><i class="fa fa-sign-out"></i> Chiqish</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { onMounted, computed } from 'vue'
import { useStore } from 'vuex'
import userService from '../../services/user.service'
import $ from 'jquery'
import authService from '../../services/auth.service'
import { useRouter } from 'vue-router'

const store = useStore()
const router = useRouter()

function checkLogin(data) {
  store.commit('setLogin', data)
}

function onLogout() {
  store.dispatch('auth/logout').then(() => {
    checkLogin(false)
  })
}

const addUserInStore = () => {
  userService.getUser().then((data) => store.commit('setUser', data))
}

const name = computed(() => {
  return store.state.user.name
})

function toggleDropDown() {
  $('#dropdownInformation').toggleClass('hidden')
}

$(document).click(function (event) {
  if (!$(event.target).closest('#dropdownBtn').length && !$(event.target).is('#dropdownBtn')) {
    $('#dropdownInformation').addClass('hidden')
  }
})

function autoLogout() {
  setTimeout(() => {
    if (!store.state.user.name) {
      authService.logout()
      checkLogin(false)
      router.go('/login')
    }
  }, 3000)
}

onMounted(() => addUserInStore(), autoLogout())
</script>

<style scoped>
.hidden {
  display: none;
  transition: all 0.5s ease-in-out;
}
#dropdownInformation {
  position: absolute;
  top: 100%;
  right: 10px;
  z-index: 10;
}
</style>