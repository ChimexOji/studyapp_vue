<template>
  <div class="home">
    <div class="hero is-link is-medium">
      <div class="hero-body has-text-centered">
        <h1 class="title">Welcome to StudyApp</h1>

        <h2 class="subtitle">An online place for learning whatever you want</h2>
      </div>
    </div>
  </div>

  <section class="section">
    <div class="container">
      <div class="columns is-multiline">
        <div class="column is-4">
          <div class="box has-text-centered">
            <span class="icon is-size-2 has-text-info">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
                <path stroke-linecap="round" stroke-linejoin="round" d="M12 6v6h4.5m4.5 0a9 9 0 11-18 0 9 9 0 0118 0z" />
              </svg>
            </span>

            <h2 class="is-size-4 mt-4 mb-4">Study at your own pace</h2>

            <p>This is just some random placeholder text</p>
          </div>
        </div>

        <div class="column is-4">
          <div class="box has-text-centered">
            <span class="icon is-size-2 has-text-info">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
                <path stroke-linecap="round" stroke-linejoin="round" d="M8.625 12a.375.375 0 11-.75 0 .375.375 0 01.75 0zm0 0H8.25m4.125 0a.375.375 0 11-.75 0 .375.375 0 01.75 0zm0 0H12m4.125 0a.375.375 0 11-.75 0 .375.375 0 01.75 0zm0 0h-.375M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
              </svg>
            </span>

            <h2 class="is-size-4 mt-4 mb-4">Study with others</h2>

            <p>This is just some random placeholder text</p>
          </div>
        </div>

        <div class="column is-4">
          <div class="box has-text-centered">
            <span class="icon is-size-2 has-text-info">
              <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6">
                <path stroke-linecap="round" stroke-linejoin="round" d="M12 21a9.004 9.004 0 008.716-6.747M12 21a9.004 9.004 0 01-8.716-6.747M12 21c2.485 0 4.5-4.03 4.5-9S14.485 3 12 3m0 18c-2.485 0-4.5-4.03-4.5-9S9.515 3 12 3m0 0a8.997 8.997 0 017.843 4.582M12 3a8.997 8.997 0 00-7.843 4.582m15.686 0A11.953 11.953 0 0112 10.5c-2.998 0-5.74-1.1-7.843-2.918m15.686 0A8.959 8.959 0 0121 12c0 .778-.099 1.533-.284 2.253m0 0A17.919 17.919 0 0112 16.5c-3.162 0-6.133-.815-8.716-2.247m0 0A9.015 9.015 0 013 12c0-1.605.42-3.113 1.157-4.418" />
              </svg>
            </span>

            <h2 class="is-size-4 mt-4 mb-4">Study from wherever</h2>

            <p>This is just some random placeholder text</p>
          </div>
        </div>

        <div class="column is-12 has-text-centered">
          <a href="#" class="button is-info is-size-3 mt-6 mb-6">Click to get started</a>
        </div>

        <hr>

        <div class="column is-3" v-for="course in courses" v-bind:key="course.id">
            <CourseItem :course="course"/>
        </div>

      </div>
    </div>
  </section>
</template>

<script>
import axios from 'axios'

import CourseItem from '@/components/CourseItem.vue'

export default {
  name: 'HomeView',
  data() {
       return {
        courses: []
       } 
    },
  components: {
      CourseItem
  },
  mounted() {
      console.log('mounted')

      axios
          .get('/api/v1/courses/get_frontpage_courses/')
          .then(response => {
              console.log(response.data)

              this.courses = response.data
          })
  }
}
</script>
