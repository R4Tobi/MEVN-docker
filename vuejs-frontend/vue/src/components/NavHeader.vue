<template>
  <div class="nav">
    <div class="left">
      <button
        class="menu"
        onclick="this.classList.toggle('opened');this.setAttribute('aria-expanded', this.classList.contains('opened')); document.getElementById('menu').classList.toggle('opened')"
        aria-label="Main Menu"
      >
        <svg width="50" height="50" viewBox="0 0 100 100">
          <path
            class="line line1"
            d="M 20,29.000046 H 80.000231 C 80.000231,29.000046 94.498839,28.817352 94.532987,66.711331 94.543142,77.980673 90.966081,81.670246 85.259173,81.668997 79.552261,81.667751 75.000211,74.999942 75.000211,74.999942 L 25.000021,25.000058"
          />
          <path class="line line2" d="M 20,50 H 80" />
          <path
            class="line line3"
            d="M 20,70.999954 H 80.000231 C 80.000231,70.999954 94.498839,71.182648 94.532987,33.288669 94.543142,22.019327 90.966081,18.329754 85.259173,18.331003 79.552261,18.332249 75.000211,25.000058 75.000211,25.000058 L 25.000021,74.999942"
          />
        </svg>
      </button>
    </div>
    <div id="menu" class="right closed">
      <ul>
        <li><router-link to="/profile">Profil</router-link></li>
        <li v-if="loggedIn"><router-link to="/home" @click="init(); logout();">Logout</router-link></li>
        <li v-if="!loggedIn"><router-link to="/home" @click="init();">Login</router-link></li>
      </ul>
    </div>
  </div>
</template>

<script>
import User from "@/scripts/User.js";
import Cookie from "../scripts/Cookie.js";

export default {
  name: "NavHeader",
  data: () => {
    return {
      loggedIn: false,
    };
  },
  methods: {
    init() {
      this.checkAuthCookie();
    },
    checkAuthCookie() {
      const cookie = new Cookie().getCookie("sessionID");
      if (cookie !== "") {
        this.loggedIn = true;
      }
    },
    logout() {
      new User().logout();
      console.log(new Cookie().getCookie("sessionID"));
      new Cookie().deleteCookie("sessionID");
      this.loggedIn = false;
    },
  },
  mounted() {
    this.init();
  },
  watch: {
    $route() {
      this.checkAuthCookie();
    },
  },

}


</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
@import "../stylesheets/nav.scss";
</style>
