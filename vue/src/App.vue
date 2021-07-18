<template>
  <div id="wrapper" class="container-fluid">
    <header>
      <nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark">
        <div class="container-fluid">
          <router-link to="/" class="navbar-brand" href="#">Djackets</router-link>
          <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarCollapse"
            aria-controls="navbarCollapse" aria-expanded="false" aria-label="Toggle navigation"
            @click="showMobileMenu = !showMobileMenu">
            <span class="navbar-toggler-icon"></span>
          </button>
          <div class="collapse navbar-collapse" id="navbarCollapse" :class="{'show': showMobileMenu }">
            <ul class="navbar-nav me-auto mb-2 mb-md-0">
              <li>
                <form class="d-flex" action="/search">
                  <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search" name="query">
                  <button class="btn btn-success me-2" type="submit">Search</button>
                </form>
              </li>
              <li class="nav-item">
                <router-link to="/summer" class="nav-link" aria-current="page" href="#">Summer</router-link>
              </li>
              <li class="nav-item">
                <router-link to="/winter" class="nav-link" aria-current="page" href="#">Winter</router-link>
              </li>
            </ul>
            <template v-if="$store.state.isAuthenticated">
              <router-link to="/my-account" class="btn btn-light me-2">My Account</router-link>
            </template>
            <template v-else>
              <router-link to="/log-in" class="btn btn-light me-2">Log in</router-link>
            </template>
            
            <router-link to="/cart" class="btn btn-success">
              <span class="icon">
                <i class="fas fa-shopping-cart">

                </i>
              </span>
              <span>Cart ({{cartTotalLength}})</span>
            </router-link>
          </div>
        </div>
      </nav>
    </header>

    <div class="is-loading-bar has-text-centered" v-bind:class="{'is-loading': $store.state.isLoading }">
      <div class="lds-dual-ring"></div>
    </div>

    <main role="main">
      <div class="container mt-5">
        <router-view />
      </div>
      <footer class="container">
        <p class="float-right"><a href="#">Back to top</a></p>
        <p>&copy; 2021 Company, Inc. &middot; <a href="#">Privacy</a> &middot; <a href="#">Terms</a></p>
      </footer>
    </main>
  </div>
</template>

<style lang="scss">
  @import '../node_modules/bootstrap/scss/bootstrap.scss';
</style>

<style lang="scss">
 @import '../node_modules/bulma';

.lds-dual-ring {
  display: inline-block;
  width: 80px;
  height: 80px;
}
.lds-dual-ring:after {
  content: " ";
  display: block;
  width: 64px;
  height: 64px;
  margin: 8px;
  border-radius: 50%;
  border: 6px solid #ccc;
  border-color: #ccc transparent #ccc transparent;
  animation: lds-dual-ring 1.2s linear infinite;
}
@keyframes lds-dual-ring {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
.is-loading-bar {
  height: 0;
  overflow: hidden;
  -webkit-transition: all 0.3s;
  transition: all 0.3s;
  &.is-loading {
    height: 80px;
  } 
}
</style>

<script>
import axios from 'axios'

  export default {
    data() {
      return {
        showMobileMenu: false,
        cart: {
          items: []
        }
      }
    },
    beforeCreate() {
      this.$store.commit('initStore')

      const token = this.$store.state.token  
      
      if (token) {
        axios.defaults.headers.common["Authorization"] = "Token " + token
      } else {
        axios.defaults.headers.common['Authorization'] = ""
      }
    },
    mounted() {
      this.cart = this.$store.state.cart
    },
    computed: {
      cartTotalLength() {
        let totalLength = 0
        for (let i = 0; i < this.cart.items.length; i++) {
          totalLength += this.cart.items[i].quantity
        }
        return totalLength
      }
    },
    watch: {
      '$store.state.cart': function() {
        this.cart = this.$store.state.cart
      }
    }
  }
</script>