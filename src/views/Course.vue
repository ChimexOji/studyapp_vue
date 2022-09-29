<template>
    <div class="courses">
        <div class="hero is-link">
            <div class="hero-body has-text-centered">
                <h1 class="title">{{ course.title }}</h1>
            </div>
        </div>

        <section class="section">
            <div class="container">
                <div class="columns content">
                    <div class="column is-2">
                        <h2>Table of contents</h2>

                        <ul>
                            <li
                                v-for="lesson in lessons"
                                v-bind:key="lesson.id"
                            >
                                <a @click="activeLesson = lesson">{{ lesson.title }}</a></li>
                        </ul>
                    </div>

                    <div class="column is-10">
                        <template v-if="$store.state.user.isAuthenticated">
                            <template v-if="activeLesson">
                                <h2>{{ activeLesson.title }}</h2>
                                {{ activeLesson.long_description }}
                            </template>

                            <template v-else>
                                <p>
                                    Lorem ipsum dolor sit amet consectetur adipisicing elit. 
                                    Ratione alias voluptates quos itaque ipsum vero, 
                                    natus omnis minus unde ab dolor, 
                                    tempore quia accusantium quam eius laborum repellat 
                                    inventore pariatur numquam mollitia quaerat. 
                                    In dolores provident quasi perferendis iste distinctio.
                                </p>
                            </template>
                        </template>

                        <template v-else>
                            <h2>Restricted access</h2>

                            <p>You need to have an account to continue!</p>
                        </template>
                    </div>
                </div>
            </div>
        </section>
    </div>
</template>

<script>
    import axios from 'axios'
    
    export default {
        data() {
           return {
            course: {},
            lessons: [],
            activeLesson: null
           } 
        },
        // vue life cycle hook to display courses backend components
        mounted() {
            console.log('mounted')

            const slug = this.$route.params.slug
    
            axios
                .get(`/api/v1/courses/${slug}/`)
                .then(response => {
                    console.log(response.data)
    
                    this.course = response.data.course
                    this.lessons = response.data.lessons
                })
        }
    }
    </script>