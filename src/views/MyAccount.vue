<template>
    <div class="page-my-account">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">My Account</h1>
            </div>

            <div class="column is-12">
                <button @click="logout()" class="button is-danger">Log Out</button>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';


export default {
    name: "MyAccount",
    methods: {
        logout() {
            axios.defaults.headers.common["Authorization"] = ""

            localStorage.removeItem("token")
            localStorage.removeItem("username")
            localStorage.removeItem("userid")

            this.$store.commit('removetoken')

            this.$router.push('/')
        }
    }
}
</script>