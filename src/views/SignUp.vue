<template>
    <div class="page-sign-up">
        <div class="columns">
            <div class="column is-4 is-offset-4">
                <h1 class="title">Sign Up</h1>

                <form @submit.prevent="submitForm">
                    <div class="field">
                        <label>Username</label>
                        <div class="control">
                            <input type="text" v-model="username" class="input">
                        </div>
                    </div>

                    <div class="field">
                        <label>Password</label>
                        <div class="control">
                            <input type="password" v-model="password" class="input">
                        </div>
                    </div>

                    <div class="field">
                        <label>Repeat Password</label>
                        <div class="control">
                            <input type="password" v-model="password2" class="input">
                        </div>
                    </div>

                    <div class="notification is-danger" v-if="errors.length">
                        <p v-for="error in errors" v-bind:key="error">{{ error }}</p>
                    </div>

                    <div class="field">
                        <div class="control">
                            <button class="button is-dark">Sign Up</button>
                        </div>
                    </div>

                    <hr>

                    Or <router-link to="/log-in">Click Here</router-link> to log in!
                </form>
            </div>
        </div>
    </div>
</template>

<script>

import axios from 'axios';
import { toast } from 'bulma-toast';
import { log } from 'util';

export default {
    name: 'SignUp',
    data() {
        return {
            username: '',
            password: '',
            password2: '',
            errors: []
        }
    },
    methods: {
        submitForm() {
            this.errors = []

            if (this.username === '') {
                this.errors.push('Please enter a username dumbass')
            }

            if (this.password === '') {
                this.errors.push('Please enter a password dumbass')
            }

            if (this.password !== this.password2) {
                this.errors.push('Passwords do not match dumbass')
            }

            if (!this.errors.length) {
                const formData = {
                   username: this.username,
                    password: this.password
                }

                axios
                    .post("/api/v1/users/", formData)
                    .then(response => {
                        toast({
                            message: "Account created, please log in!",
                            type: 'is-success',
                            dismissible: true,
                            duration: 2000,
                            pauseOnHover: true,
                            position: 'bottom-right'
                        })

                        this.$router.push('/log-in')
                    })
                    .catch(error => {
                        if (error.response) {
                            for (const property in error.response.data) {
                                this.errors.push(`${property}: ${error.response.data[property]}`)
                            }

                            console.log(JSON.stringify(error.response.data))
                        } else if (error.message) {
                            this.errors.push("Something went wrong nigga! Please don't try later")

                            console.log(JSON.stringify(error))
                        }
                    })
            }
        }
    }
}


</script>