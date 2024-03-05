<template>
  <div class="container mx-auto py-8">
    <div class="max-w-md mx-auto bg-gray-800 p-5 rounded-lg shadow-md">
      <div class="mb-4">
        <input
          v-model="state.searchTerm"
          type="text"
          placeholder="Search users..."
          class="w-full border-gray-600 bg-gray-700 text-white rounded-md shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
        />
      </div>
      
      <div v-if="state.loading">
        <div
          class="inline-block h-8 w-8 animate-spin rounded-full border-4 border-solid border-current border-e-transparent align-[-0.125em] text-surface motion-reduce:animate-[spin_1.5s_linear_infinite] dark:text-white"
          role="status">
          <span
            class="!absolute !-m-px !h-px !w-px !overflow-hidden !whitespace-nowrap !border-0 !p-0 ![clip:rect(0,0,0,0)]"
            >Loading...</span
          >
        </div>
      </div>
      <ul v-else>
        <li
          v-for="(user, index) in renderedUsers"
          :key="index"
          class="flex items-center py-2 border-b border-gray-700"
        >
          <img
            alt="User"
            class="w-10 h-10 rounded-full mr-3"
            v-bind:src="user.avatar_url"
          />
          <span class="text-gray-300">{{user.login}}</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import {
  computed,
  onMounted,
  reactive,
} from 'vue';

const state = reactive({
  users: [],
  loading: false,
  searchTerm: '',
});

const loadUsers = async () => {
  state.loading = true;
  try {
    const res = await fetch('https://api.github.com/users');
    const data = await res.json();
    state.users = data;
  } finally {
    state.loading = false;
  }
};

const renderedUsers = computed(() => {
  if (state.searchTerm.length > 0) {
    return state.users.filter((user) => {
      return state.searchTerm.toLowerCase().split(' ').every((v) => user.login.toLowerCase().includes(v));
    });
  }
  return state.users;
});

onMounted(loadUsers);

</script>

<style>
</style>
