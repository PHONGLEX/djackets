<template>
    <div class="text-center">
        <form class="form-signin" @submit.prevent="submitForm">
            <h1 class="h3 mb-3 font-weight-normal">Log In</h1>
            <label for="inputEmail" class="sr-only">Email address</label>
            <input type="email" id="inputEmail" class="form-control" placeholder="Email address" required autofocus
                v-model="username">
            <label for="inputPassword" class="sr-only">Password</label>
            <input type="password" id="inputPassword" class="form-control mt-2" placeholder="Password" required
                v-model="password">
            
            <button class="btn btn-lg btn-primary btn-block mt-2" type="submit">Log in</button>
            <div class="alert alert-danger mt-2" role="alert" v-if="errors.length">
                <p v-for="error in errors" v-bind:key="error">{{error}} <br></p>
            </div>

            Or <router-link to="/sign-up">click here</router-link> to sign up
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

export default {
    name: "LogIn",
    data() {
        return {
            "username": "",
            "password": "",
            "errors": []
        }
    },
    mounted() {
        document.title = "Log In | Djackets"
    },
    methods: {
        async submitForm() {
            axios.defaults.headers.common["Authorization"] = ""

            localStorage.removeItem("token")

            const formData = {
                username: this.username,
                password: this.password
            }

            await axios.post("/api/v1/token/login/", formData)
            .then(response => {
                const token = response.data.auth_token

                this.$store.commit('setToken', token)

                axios.defaults.headers.common["Authorization"] = "Token " + token

                localStorage.setItem("token", token)

                const toPath = this.$route.query.to  || "/cart"

                this.$router.push(toPath)
            })
            .catch(error => {
                if (error.response) {
                    for (const property in error.response.data) {
                        this.errors.push(`${propery}: ${error.response.data["property"]}`)
                    }
                } else {
                    this.errors.push("Something went wrong, Please try again!")

                    console.log(JSON.stringify(error))
                }
            })
        }
    }
}
</script>