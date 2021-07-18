<template>
    <div class="text-center">
        <form class="form-signin" @submit.prevent="submitForm">
            <h1 class="h3 mb-3 font-weight-normal">Sign up</h1>
            <label for="inputEmail" class="sr-only">Email address</label>
            <input type="email" id="inputEmail" class="form-control" placeholder="Email address" required autofocus
                v-model="username">
            <label for="inputPassword" class="sr-only">Password</label>
            <input type="password" id="inputPassword" class="form-control mt-2" placeholder="Password" required
                v-model="password">
            <label for="inputPassword" class="sr-only">Password Again</label>
            <input type="password" id="inputPassword" class="form-control mt-2" placeholder="Password Repeat" required
                v-model="password2">

            <button class="btn btn-lg btn-primary btn-block" type="submit">Sign up</button>
            <div class="alert alert-danger mt-2" role="alert" v-if="errors.length">
                <p v-for="error in errors" v-bind:key="error">{{error}} <br></p>
            </div>
            <hr>
            Or <router-link to="/log-in">click here</router-link> to log in
        </form>
    </div>

</template>

<style scoped>
    .form-signin {
        width: 100%;
        max-width: 330px;
        padding: 15px;
        margin: auto;
    }

    .form-signin .checkbox {
        font-weight: 400;
    }

    .form-signin .form-control {
        position: relative;
        box-sizing: border-box;
        height: auto;
        padding: 10px;
        font-size: 16px;
    }

    .form-signin .form-control:focus {
        z-index: 2;
    }

    .form-signin input[type="email"] {
        margin-bottom: -1px;
        border-bottom-right-radius: 0;
        border-bottom-left-radius: 0;
    }

    .form-signin input[type="password"] {
        margin-bottom: 10px;
        border-top-left-radius: 0;
        border-top-right-radius: 0;
    }
</style>

<script>
    import axios from 'axios'
    import {
        toast
    } from 'bulma-toast'

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

                if (this.password !== this.password2) {
                    this.errors.push("The passwords doesn\'t match")
                }

                if (!this.errors.length) {
                    const formData = {
                        username: this.username,
                        password: this.password
                    }

                    axios.post('/api/v1/users/', formData)
                    .then(response => {
                        toast({
                            message: "Account created, please log in!",
                            type: 'is-success',
                            dismissable: true,
                            pauseOnHover: true,
                            duration: 2000,
                            position: 'top-right'
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
                            this.errors.push("Something went wrong. Please try again")

                            console.log(JSON.stringify(error))
                        }
                    })
                }

            }
        }
    }
</script>