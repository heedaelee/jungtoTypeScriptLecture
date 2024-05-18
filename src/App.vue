<template>
  <div id="app">
    <h1>깃헙 유저 서치 엔진</h1>
    <input
      v-model="username"
      placeholder="깃헙 아이디를 입력하세요"
      @keyup.enter="searchUser"
    />
    <div v-if="user">
      <h2>{{ user.name }}</h2>
      <p>{{ user.bio }}</p>
      <img :src="user.avatar_url" alt="User Avatar" />
      <p>
        <a :href="user.html_url" target="_blank">프로필 보기</a>
      </p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import axios from "axios";

/**
 * TODO: 타입을 걸어봅시다!
 */

interface IgitHubUser {
  name: string;
  bio: string;
  avatar_url: string;
  html_url: string;
}

const username = ref<string>("");
const user = ref<IgitHubUser | null>(null);

const searchUser = async () => {
  if (username.value.trim() !== "") {
    try {
      const response = await axios.get<IgitHubUser>(
        `https://api.github.com/users/${username.value}`
      );
      console.log(`response`, response);
      user.value = response.data;
    } catch (error) {
      console.error("에러 발생", error);
      user.value = null;
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
