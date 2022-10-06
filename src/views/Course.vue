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
                                <a @click="setActiveLesson(lesson)">{{ lesson.title }}</a></li>
                        </ul>
                    </div>

                    <div class="column is-10">
                        <template v-if="$store.state.user.isAuthenticated">
                            <template v-if="activeLesson">
                                <h2>{{ activeLesson.title }}</h2>
                                {{ activeLesson.long_description }}

                               <hr>

                               <article class="media box" v-for="comment in comments" v-bind:key="comment.id">
                                    <div class="media-content">
                                        <div class="content">
                                            <p>
                                                <strong>{{ comment.name }}</strong>
                                                <br>
                                                {{ comment.content }}<br>
                                                <time>{{ comment.created_at }}</time>
                                                
                                            </p>
                                        </div>
                                    </div>
                               </article>

                               <form v-on:submit.prevent="submitComment()">
                                    <div class="field">
                                        <label class="label">Name</label>
                                        <div class="control">
                                            <input type="text" class="input" v-model="comment.name">
                                        </div>
                                    </div>

                                    <div class="field">
                                        <label class="label">Content</label>
                                        <div class="control">
                                            <textarea class="textarea" v-model="comment.content"></textarea>
                                        </div>
                                    </div>

                                    <div class="field">
                                        <div class="control">
                                            <button class="button is-link">Submit</button>
                                        </div>
                                    </div>
                               </form>
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
                comments: [],
                activeLesson: null,
                comment: {
                    name : '',
                    content: ''
                }
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
        },
        methods: {
            submitComment() {
                console.log('submitComment')

                axios
                    .post(`/api/v1/courses/${this.course.slug}/${this.activeLesson.slug}/`, this.comment)
                    .then(response => {
                        // resets the form
                        this.comment.name = ''
                        this.comment.content = ''
                        alert('The comment was added!')
                    })
                    .catch(error => {
                        console.log(error)
                    })
            },
            setActiveLesson(lesson) {
                this.activeLesson = lesson

                this.getComments()
            },
            getComments() {
                axios
                    .get(`/api/v1/courses/${this.course.slug}/${this.activeLesson.slug}/get-comments/`)
                    .then(response => {
                        console.log(response.data)

                        this.comments = response.data
                    })
            }
        }
    }
    </script>