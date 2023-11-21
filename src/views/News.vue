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
const like = ref(0)
const love = ref(0)
const sad = ref(0)
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
  news.value = res.data.news
  like.value = res.data.like
  love.value = res.data.love
  sad.value = res.data.sad
  comments.value = res.data.comments
  console.log(res.data)
}
onBeforeMount(getData)
</script>
<template>
  <AppHead title="News Web Page" />
  <div id="content">
    <h1 class="display-4 fst-italic">{{ news[0].title }}</h1>
    <p class="lead my-3">{{ news[0].content }}</p>
    <p class="blog-post-meta">{{ news[0].news_date }} by <a href="#">{{ news[0].user.name }}</a></p>
        <p>
            <i class="bi bi-hand-thumbs-up padding_right">
                {{ like }}
              </i>
              <i class="bi bi bi-heart padding_right">
              {{ love }}
              </i>
              <i class="bi bi-hand-thumbs-down padding_right">
              {{ sad }}
              </i>
        </p>
        <div v-for="comment in comments">
                        <div class="card w-75 mb-3" style="background-color: #D6DCD7; width: 800px;">
                            <div class="card-body">
                                <p class="card-text">{{ comment.comment }}</p>
                                              <br />
              <i class="bi bi-hand-thumbs-up" style="padding-right: 10px">

              </i>
              <i class="bi bi bi-heart" style="padding-right: 10px">

              </i>
              <i class="bi bi-hand-thumbs-down" style="padding-right: 10px">

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
