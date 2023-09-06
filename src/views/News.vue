<script setup>
import { ref, onMounted, onBeforeMount } from 'vue'
import { RouterLink, RouterView, useRoute } from 'vue-router'
import axios from 'axios'

const route = useRoute()
const localToken = ref('no_token')
const news = ref([]);
const breaking_news = ref(0);
const featured_section = ref([1, 2])
const news_month_year = ref([2000, 20001,2002])
const root_link = ref('http://127.0.0.1:8000')
const link = ref(`${ root_link.value }/api/news/${ route.params.id }`)

const getData = async () => {
    const v = { "a": "a" }
    localToken.value = localStorage.getItem('token')
    const res = await axios.get(link.value, v,
      {
        headers: {
            Accept: 'application/json',
            'Content-Type': 'application/json',
            Authorization: `Bearer ${ localToken.value }`
        }
      }
      
      );
      news.value = res.data.news;
      console.log(res.data);
  }
  onBeforeMount(
    getData
  );
</script>
<template>
    <AppHead title="News Web Page" />
    <div id="content">
      <h1 class="display-4 fst-italic">{{ news[0].title }}</h1>
      <p class="lead my-3">{{ news[0].content }}</p>
    </div>
</template>
<style scoped>
#content {
    width: 80%;
    margin: auto;
    padding: 10px;
}
</style>
