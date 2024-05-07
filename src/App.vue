<script setup>
//vue Librarys or externals
import { computed, onMounted, ref } from 'vue';

//vue components
import PaginatePost from './components/PaginatePost.vue'
import BlogPost from './components/BlogPost.vue'
import LoadingSpinner from './components/LoadingSpinner.vue'

var title = 'Wekeend Posts '
const posts = ref([]);
const favorite = ref("");

const postperpage = 10
const start = ref(0)
const end = ref(postperpage)

const loading = ref(true);

const next = () => {
  start.value += + postperpage
  end.value += + postperpage
}

const previous = () => {
  start.value += - postperpage
  end.value += - postperpage
}

const changeFavorite = (title) => {
  favorite.value = title;
}

// onMounted(async () => {
//   loading.value = true
//   try {
//     const res = await fetch("https://jsonplaceholder.typicode.com/posts")
//     posts.value = await res.json()
//   } catch (error) {
//     console.log(error)
//   } finally {
//     setTimeout(() => {
//       loading.value = false
//     }, 1200)
//   }
// });

// fetch("https://jsonplaceholder.typicode.com/posts")
//   .then((res) => res.json())
//   .then((data) => {
//     posts.value = data;
//   })
//   .catch(e => console.log(e))
//   .finally(() => {
//     setTimeout(() => {
//       loading.value = false;
//     }, 1200);
//   })

const fetchData = async () => {
  try {
    const res = await fetch("https://jsonplaceholder.typicode.com/posts")
    posts.value = await res.json()
  } catch (error) {
    console.log(error)
  } finally {
    setTimeout(() => {
      loading.value = false
    }, 2000);
  }
}

fetchData()

const maxLength = computed(() => posts.value.length)

</script>

<template>
  <LoadingSpinner v-if="loading"></LoadingSpinner>
  <div class="container" v-else>
    <h1>{{ title }}</h1>
    <h2>My favorite posts: {{ favorite }}</h2>

    <PaginatePost class="mb-2" @next="next" @previous="previous" :start="start" :end="end" :maxLength="maxLength" />

    <BlogPost v-for="post in posts.slice(start, end)" :key="post.id" :title="post.title" :id="post.id" :body="post.body"
      @changeFavorite="changeFavorite" class="mb-2"></BlogPost>
  </div>


</template>
