<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container">
      <a class="navbar-brand" href="/"><ion-icon class="icon" name="home-outline"></ion-icon></a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavDropdown"
        aria-controls="navbarNavDropdown" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse" id="navbarNavDropdown">
        <div class="navbar-nav col-md-6">
          <ul class="navbar-nav me-auto mb-2 mb-lg-0">
            <div v-for="menu in menuItems">
              <li class="nav-item" v-if="menu.isLoggedIn">
                <router-link class="nav-link" :to="menu.to">{{
                  menu.title
                }}</router-link>
              </li>
            </div>
          </ul>
        </div>

        <div class="navbar-nav col-md-6">
          <ul class="navbar-nav ms-auto mb-2 mb-lg-0">
            <li class="nav-item" v-if="!status.loggedIn">
              <div class="dropdown">
                <a id="navbarDropdown" role="button" data-bs-toggle="dropdown" aria-expanded="false"><ion-icon
                    class="icon" name="person-outline"></ion-icon></a>
                <a href="/cart"><ion-icon class="icon" name="cart-outline"></ion-icon></a>

                <ul class="dropdown-menu" aria-labelledby="navbarDropdown">
                  <li>
                    <router-link class="dropdown-item" to="/bejelentkezes">Bejelentkezés</router-link>
                  </li>
                  <li>
                    <router-link class="dropdown-item" to="/regisztracio">Regisztráció</router-link>
                  </li>
                  <li>
                    <div class="form-check form-switch">
                      <label class="switch">
                        <input type="checkbox" @click="toggleDark()">
                        <span class="slider round"></span>
                      </label>
                      <label class="form-check-label" for="darkSwitch">Dark Mode</label>
                    </div>
                  </li>
                </ul>
              </div>
            </li>


            <div v-if="status.loggedIn">
              {{ user.name }}
              <li class="nav-item" v-if="status.loggedIn">
                <a class="nav-link" href="#" @click="onLogout">Kijelentkezés</a>
              </li>
            </div>
          </ul>
        </div>
      </div>
    </div>
  </nav>
</template>

<script setup>
import { computed } from "vue";
import { useUserStore } from "../stores/userstore";
import { storeToRefs } from "pinia";
import { useRouter } from "vue-router";
import { ref } from "vue";
import { userDark, useToggle } from "@vueuse/core";
const { status, user } = storeToRefs(useUserStore());
const { logout } = useUserStore();
const router = useRouter();

const isDark = userDark({
  selector: "body",
  attribute: "colore-scheme",
  valueDark: 'dark',
  valueLight: 'light'
})

const toggleDark = useToggle(isDark)

const menuItems = computed(() => {
  return [
    {
      title: "Nyitó oldal",
      to: "/",
      isLoggedIn: true,
    },
    {
      title: "Termékek",
      to: "/termekek",
      isLoggedIn: true,
    },
    {
      title: "Új blog",
      to: "/ujblog",
      isLoggedIn: status.value.loggedIn,
    },
    {
      title: "Kosár",
      to: "/cart",
      isLoggedIn: status.value.loggedIn,
    },
  ];
});

function onLogout() {
  logout().then(() => {
    router.push("/");
  });
}

</script>

<style lang="scss" scoped>
.icon {
  width: 30px;
  height: 30px;
  margin: 5px;
  color: black;
}

.switch {
  position: relative;
  display: inline-block;
  width: 60px;
  height: 34px;
}

/* Hide default HTML checkbox */
.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

/* The slider */
.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: .4s;
  transition: .4s;
}

.slider:before {
  position: absolute;
  content: "";
  height: 26px;
  width: 26px;
  left: 4px;
  bottom: 4px;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;
}

input:checked+.slider {
  background-color: #2196F3;
}

input:focus+.slider {
  box-shadow: 0 0 1px #2196F3;
}

input:checked+.slider:before {
  -webkit-transform: translateX(26px);
  -ms-transform: translateX(26px);
  transform: translateX(26px);
}

/* Rounded sliders */
.slider.round {
  border-radius: 34px;
}

.slider.round:before {
  border-radius: 50%;
}
</style>
