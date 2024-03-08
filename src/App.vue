<template>
  <div class="container mx-auto py-8">
    <div class="max-w-md mx-auto bg-gray-800 p-5 rounded-lg shadow-md">
      <div class="mb-4">
        <input
          v-model="searchTerm"
          type="text"
          placeholder="Search users..."
          class="w-full border-gray-600 bg-gray-700 text-white rounded-md shadow-sm focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50"
        />
      </div>

      <UsersList :searchTerm="searchTerm" />
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';
import UsersList from './components/UsersList.vue';

export default {
  name: 'App',
  components: {
    UsersList,
  },
  setup() {
    const searchTerm = ref('');

    return {
      searchTerm,
    }
  }
}
</script>

<style>
</style>
