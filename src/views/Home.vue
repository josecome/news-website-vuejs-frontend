<script setup>
import { ref, onMounted, onBeforeMount } from 'vue'
import { RouterLink, RouterView, useRoute } from 'vue-router'
import axios from 'axios'

const localToken = ref('no_token')
const news = ref([])
const breaking_news = ref(0)
const featured_section = ref([1, 2])
const news_month_year = ref([2000, 20001, 2002])
const root_link = ref('http://127.0.0.1:8000')
const link = ref(`${root_link.value}/api/`)

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
  breaking_news.value = res.data.breaking_news
  featured_section.value = res.data.featured_section
  news_month_year.value = res.data.news_month_year
  console.log(res.data)
}
onBeforeMount(getData)
</script>

<template>
  <main class="container">
    <div class="p-4 p-md-5 mb-4 rounded text-bg-dark">
      <div class="col-md-6 px-0">
        <h1 class="display-4 fst-italic">{{ news[breaking_news].title }}</h1>
        <p class="lead my-3">{{ news[breaking_news].content.slice(0, 200) + '...' }}</p>
        <p class="lead mb-0">
          <RouterLink :to="`/news/${news[breaking_news].id}`" style="color: white">
            Continue reading...
          </RouterLink>
        </p>
      </div>
    </div>
    <h3>Featured Sections</h3>
    <div class="row mb-2">
      <div class="col-md-6">
        <div
          class="row g-0 border rounded overflow-hidden flex-md-row mb-4 shadow-sm h-md-250 position-relative"
        >
          <div class="col p-4 d-flex flex-column position-static">
            <strong class="d-inline-block mb-2 text-primary">World</strong>
            <h3 class="mb-0">{{ news[featured_section[0]].title }}</h3>
            <div class="mb-1 text-muted">{{ news[featured_section[0]].news_date }}</div>
            <p class="card-text mb-auto">{{ news[featured_section[0]].content.slice(1, 100) }}</p>
            <RouterLink :to="`/news/${news[featured_section[0]].id}`">
              Continue reading...
            </RouterLink>
          </div>
          <div class="col-auto d-none d-lg-block">
            <svg
              class="bd-placeholder-img"
              width="200"
              height="250"
              xmlns="http://www.w3.org/2000/svg"
              role="img"
              aria-label="Placeholder: Thumbnail"
              preserveAspectRatio="xMidYMid slice"
              focusable="false"
            >
              <title>Placeholder</title>
              <rect width="100%" height="100%" fill="#55595c" />
              <text x="50%" y="50%" fill="#eceeef" dy=".3em">Thumbnail</text>
            </svg>
          </div>
        </div>
      </div>
      <div class="col-md-6">
        <div
          class="row g-0 border rounded overflow-hidden flex-md-row mb-4 shadow-sm h-md-250 position-relative"
        >
          <div class="col p-4 d-flex flex-column position-static">
            <strong class="d-inline-block mb-2 text-success">Most read this week</strong>
            <h3 class="mb-0">{{ news[featured_section[0]].title }}</h3>
            <div class="mb-1 text-muted">{{ news[featured_section[0]].news_date }}</div>
            <p class="card-text mb-auto">{{ news[featured_section[0]].content.slice(1, 100) }}</p>
            <RouterLink :to="`/news/${news[featured_section[0]].id}`">
              Continue reading...
            </RouterLink>
          </div>
          <div class="col-auto d-none d-lg-block">
            <svg
              class="bd-placeholder-img"
              width="200"
              height="250"
              xmlns="http://www.w3.org/2000/svg"
              role="img"
              aria-label="Placeholder: Thumbnail"
              preserveAspectRatio="xMidYMid slice"
              focusable="false"
            >
              <title>Placeholder</title>
              <rect width="100%" height="100%" fill="#55595c" />
              <text x="50%" y="50%" fill="#eceeef" dy=".3em">Thumbnail</text>
            </svg>
          </div>
        </div>
      </div>
    </div>

    <div class="row g-5">
      <div class="col-md-8">
        <h3 class="pb-4 mb-4 fst-italic border-bottom">Other News</h3>

        <article class="blog-post">
          <div v-for="newss in news" :key="id">
            <h4 class="blog-post-title mb-1">{{ newss.title }}</h4>
            <span>{{ newss.content.slice(0, 200) + '...' }}</span>
            <RouterLink :to="`/news/${newss.id}`"> Continue reading... </RouterLink>
            <p class="blog-post-meta">{{ newss.news_date }} by <a href="#">Jose</a></p>
          </div>
        </article>
        <nav class="blog-pagination" aria-label="Pagination">
          <a class="btn btn-outline-primary rounded-pill" href="#">Older</a>
          <a class="btn btn-outline-secondary rounded-pill disabled">Newer</a>
        </nav>
      </div>

      <div class="col-md-4">
        <div class="position-sticky" style="top: 2rem">
          <div class="p-4 mb-3 bg-light rounded">
            <h4 class="fst-italic">About</h4>
            <p class="mb-0">
              Customize this section to tell your visitors a little bit about your publication,
              writers, content, or something else entirely. Totally up to you.
            </p>
          </div>

          <div class="p-4">
            <h4 class="fst-italic">Old News</h4>
            <ol class="list-unstyled mb-0">
              <li v-for="news_month_years in news_month_year">
                <a href="#">{{ news_month_years }}</a>
              </li>
            </ol>
          </div>

          <div class="p-4">
            <h4 class="fst-italic">Elsewhere</h4>
            <ol class="list-unstyled">
              <li><a href="#">GitHub</a></li>
              <li><a href="#">Twitter</a></li>
              <li><a href="#">Facebook</a></li>
            </ol>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style>
@import 'bootstrap/dist/css/bootstrap.css';
@import 'bootstrap-icons/font/bootstrap-icons.css';
</style>

<style>
/* stylelint-disable selector-list-comma-newline-after */

.blog-header {
  border-bottom: 1px solid #e5e5e5;
}

.blog-header-logo {
  font-family: 'Playfair Display', Georgia, 'Times New Roman',
    serif /*rtl:Amiri, Georgia, "Times New Roman", serif*/;
  font-size: 2.25rem;
}

.blog-header-logo:hover {
  text-decoration: none;
}

h1,
h2,
h3,
h4,
h5,
h6 {
  font-family: 'Playfair Display', Georgia, 'Times New Roman',
    serif /*rtl:Amiri, Georgia, "Times New Roman", serif*/;
}

.display-4 {
  font-size: 2.5rem;
}
@media (min-width: 768px) {
  .display-4 {
    font-size: 3rem;
  }
}

.flex-auto {
  flex: 0 0 auto;
}

.h-250 {
  height: 250px;
}
@media (min-width: 768px) {
  .h-md-250 {
    height: 250px;
  }
}

/* Pagination */
.blog-pagination {
  margin-bottom: 4rem;
}

/*
 * Blog posts
 */
.blog-post {
  margin-bottom: 4rem;
}
.blog-post-title {
  font-size: 2.5rem;
}
.blog-post-meta {
  margin-bottom: 1.25rem;
  color: #727272;
}

/*
 * Footer
 */
.blog-footer {
  padding: 2.5rem 0;
  color: #727272;
  text-align: center;
  background-color: #f9f9f9;
  border-top: 0.05rem solid #e5e5e5;
}
.blog-footer p:last-child {
  margin-bottom: 0;
}
</style>
