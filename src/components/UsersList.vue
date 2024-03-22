<template>
  <div>
    <div v-if="loading">
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
        v-for="(user, index) in users"
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
</template>

<script setup>
import {
  defineProps,
  onMounted,
  ref,
  watch,
} from 'vue';

const props = defineProps({
  searchTerm: {
    type: String,
    default: '',
  }
});

let debounceTimer = null;

const loading = ref(false);
const users = ref([]);

const loadUsers = async () => {
  loading.value = true;
  let data;
  try {
    if (props.searchTerm.length > 0) {
      const res = await fetch(`https://api.github.com/search/users?q=${props.searchTerm}&per_page=10`);
      const json = await res.json();
      data = json.items;
    } else {
      const res = await fetch('https://api.github.com/users');
      data = await res.json();
    }
    users.value = data;
  } finally {
    loading.value = false;
  }
};

// watch(inputText.value, (next, prev) =>{
//   do something
// })
watch(() => props.searchTerm, async (newValue, oldValue) => {
  if (newValue !== oldValue) {
    if (debounceTimer) {
      clearTimeout(debounceTimer);
    }
    debounceTimer = setTimeout(async () => {
      await loadUsers();
    }, 250);
  }
})

onMounted(() => {
  loadUsers();
  // do some other stuff, see before for simple callback
})
// onMounted(loadUsers);
</script>

<style>
</style>
