<script setup>
import { ref, onMounted, onBeforeMount } from 'vue'
import { RouterLink, RouterView, useRoute } from 'vue-router'
import axios from 'axios'

const route = useRoute()
const localToken = ref('no_token')
const news = ref([])
const breaking_news = ref(0)
const featured_section = ref([1, 2])
const news_month_year = ref([2000, 20001, 2002])
const root_link = ref('http://127.0.0.1:8000')
const link = ref(`${root_link.value}/api/news/${route.params.id}`)
const news_like = ref(0)
const news_love = ref(0)
const news_sad = ref(0)
const comments = ref([])

const getData = async () => {
  const v = { a: 'a' }
  localToken.value = localStorage.getItem('token')
  const res = await axios.get(link.value, v, {
    headers: {
      Accept: 'application/json',
      'Content-Type': 'application/json',
      Authorization: `Bearer ${localToken.value}`
    }
  })
  news.value = res.data.data
  news_like.value = Object.keys(res.data.data.likes.filter(v => v.type === 'like')).length;
  news_love.value = Object.keys(res.data.data.likes.filter(v => v.type === 'love')).length;
  news_sad.value = Object.keys(res.data.data.likes.filter(v => v.type === 'sad')).length;
  comments.value = res.data.data.comments
  console.log('================')
  console.log(res.data)
}
onBeforeMount(getData)
</script>
<template>
  <AppHead title="News Web Page" />
  <div id="content">
    <h1 class="display-4 fst-italic">{{ news.title }}</h1>
    <p class="lead my-3">{{ news.content }}</p>
    <p class="blog-post-meta">{{ news.news_date }} by <a href="#">{{ news.user }}</a></p>
        <p>
            <i class="bi bi-hand-thumbs-up padding_right">
                {{ news_like }}
              </i>
              <i class="bi bi bi-heart padding_right">
              {{ news_love }}
              </i>
              <i class="bi bi-hand-thumbs-down padding_right">
              {{ news_sad }}
              </i>
        </p>
        <div v-for="comment in comments">
                        <div class="card w-75 mb-3" style="background-color: #D6DCD7; width: 800px;">
                            <div class="card-body">
                                <p class="card-text">{{ comment.comment }}</p>
                                              <br />
              <i class="bi bi-hand-thumbs-up" style="padding-right: 10px">
                {{ Object.keys(comment.likes.filter((like) => like.type === 'like')).length }}
              </i>
              <i class="bi bi bi-heart" style="padding-right: 10px">
                {{ Object.keys(comment.likes.filter((like) => like.type === 'love')).length }}
              </i>
              <i class="bi bi-hand-thumbs-down" style="padding-right: 10px">
                {{ Object.keys(comment.likes.filter((like) => like.type === 'sad')).length }}
              </i>
                            </div>
                        </div>
        </div>
  </div>
</template>
<style scoped>
#content {
  width: 80%;
  margin: auto;
  padding: 10px;
}
</style>
