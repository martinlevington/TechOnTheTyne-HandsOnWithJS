# Part 7 Users and Authentication

We are going to let registered user order from our online store.  To do this we need to add the functionality to our application so that users can register and login.  We will hook into Strapi's Users & Permissions plugin as this already available in the project.

In the frontend application we will install the strapi SDK.

make sure that your terminal is in the correct folder:

    /frontend

Now install the Strapi SDK:

    yarn add strapi-sdk-javascript

Create the following file, so that we can add in the required functionality.

```
    /frontend/store/Strapi.js
```

Copy and paste in the following code:

```js
import Strapi from 'strapi-sdk-javascript/build/main'

const apiUrl = process.env.API_URL || 'http://localhost:1337'  
const strapi = new Strapi(apiUrl)

export default strapi;  
export { apiUrl }
```

We are now going to use the js-cookie package to store our user data.

Install the js-cookie as follows:

```
    yarn add js-cookie
```

Create a file called auth.js as follows:

    /frontend/store/auth.js 

Copy and paste in the following code:

```js
import Cookies from 'js-cookie'

// Init empty state
export const state = () => {}

// Create a mutation to save data 'user' in a cookie
export const state = () => {}

export const mutations = {  
  setUser(state, user) {
    state.user = user
    Cookies.set('user', user)
  },

  logout(state) {
    state.user = null
    Cookies.set('user', null)
  }
}

// get the current username from the state
export const getters = {  
  username: state => {
    return state.user && state.user.username
  }
}
```

Now lets create a registration page so that users can register.

Create a new file:

```
    /frontend/pages/auth/register.vue
```

Copy/paste in the following code:

```html
<template>  
<div>

  <div class="uk-child-width-1-2@m uk-grid uk-grid-match" uk-height-match=".register-card">
     <div>
          <div class="uk-card uk-card-default uk-card-body register-card"  >
            <img src="/TechOnTheTyne.jpeg" height="500" width="500" class="uk-align-center" alt="">
          </div>
     </div>
     <div>
          <div class="uk-card uk-card-default uk-card-large uk-card-body register-card">

              <form @submit.stop.prevent="handleSubmit">
                  <fieldset class="uk-fieldset">

                      <legend class="uk-legend">Register</legend>

                      <div class="uk-margin">
                        <label class="uk-form-label">Username:</label>
                        <input class="uk-input" v-model="username" type="email" placeholder="username">
                      </div>

                      <div class="uk-margin">
                        <label class="uk-form-label" for="form-stacked-text">Password:</label>
                        <input class="uk-input" v-model="password" type="password" placeholder="password">
                      </div>

                      <div class="uk-margin">
                        <button class="uk-button uk-button-primary uk-width-1-1" :disabled="loading" type="submit">Submit</button>
                      </div>

                      <div class="uk-margin">
                        <p>
                          Already have an account?
                          <router-link :to="{ name: 'auth-signin'}">
                            Login
                          </router-link>
                        </p>
                      </div>
                  </fieldset>
              </form>
          </div>
     </div>
  </div>

</div>  
</template>

<script>  
import { mapMutations } from 'vuex'  
import Strapi from 'strapi-sdk-javascript/build/main'

const apiUrl = process.env.API_URL || 'http://localhost:1337/'
const strapi = new Strapi(apiUrl)


export default {  
  data() {
    return {
      email: '',
      password: '',
      username: '',
      loading: false
    }
  },
  methods: {
    // register a user
    async handleSubmit() {
      try {
        this.loading = true
        this.email = this.username;
        const response = await strapi.register(
          this.username, // use the email address for the user name
          this.email,
          this.password
        )
        this.loading = false
        this.setUser(response.user)
         this.$router.push('/')
      } catch (err) {
           console.log(err)
        this.loading = false
        alert(err.message || 'An error occurred.')
      }
    },
    ...mapMutations({
      setUser: 'auth/setUser'
    })
  }
}
</script>
```

Update the Header file to include links to our registration.

Open the following file:

```
    /frontend/components/Header.vue
```

```html
<template>
  <client-only>
    <nav class="uk-navbar-container" uk-navbar>
      <div class="uk-navbar-left">
        <ul class="uk-navbar-nav">
          <li class="uk-active">
            <router-link tag="a" class="navbar-brand" to="/" exact>
              <img src="/TechOnTheTyneSquare.png"  width="80" height="80"  alt="Tech on the Tyne" /> &nbsp; 
            </router-link>
          </li>
          <li>
            <router-link tag="a" class="navbar-brand" to="/takeaways" exact>Takeways</router-link>
          </li>
        </ul>
      </div>

      <div class="uk-navbar-right">
        <!-- If logged in -->
        <ul class="uk-navbar-nav" v-if="username">
           <li>
            <router-link data-uk-icon="icon:cart" tag="a" title="Basket" class="navbar-brand" to="/store/basket" exact></router-link>
          </li>
          <li>
            <a href="#" data-uk-icon="icon:user"></a>
            <div class="uk-navbar-dropdown uk-navbar-dropdown-bottom-left">
              <ul class="uk-nav uk-navbar-dropdown-nav">
                <li class="uk-nav-header uk-text-small uk-text-primary">YOUR ACCOUNT</li>
                <li>
                  <a href="#">
                    <span data-uk-icon="icon: info"></span>
                    Hi: {{ username }}
                  </a>
                </li>
                <li>
                  <a href="#">
                    <span data-uk-icon="icon: refresh"></span> Edit
                  </a>
                </li>
                <li class="uk-nav-divider"></li>
                <li>
                  <a href="#">
                    <span data-uk-icon="icon: thumbnails"></span> Your Orders
                  </a>
                </li>
                <li class="uk-nav-divider"></li>
                <li>
                  <a href="#" @click="logout">
                    <span data-uk-icon="icon: sign-out"></span> Logout
                  </a>
                </li>
              </ul>
            </div>
          </li>
          <li class="uk-width-small uk-visible@m"><div></div></li>
        </ul>

        <!-- If logged out -->
        <ul class="uk-navbar-nav" v-else>
          <li>
             <router-link :to="{ name: 'auth-register'}">Register</router-link>
          </li>
          <li>
             <router-link :to="{ name: 'auth-signin'}">Sign In</router-link>
          </li>
        </ul>
      </div>
    </nav>
  </client-only>
</template>

<script>
import { mapMutations } from "vuex";

export default {
  computed: {
    username() {
      return this.$store.getters["auth/username"];
    }
  },
  methods: {
    ...mapMutations({
      logout: "auth/logout"
    })
  }
};
</script>  
```

## Cookies Server Side

Since Nuxt.js give our application the ability to render code server side, we need to be enable to access to the cookies, on the server side too. This can be achieved using the 'nuxtServerInit' action which is invoked when the Nuxt.js server starts.

First we need to install 'cookieparser'

In you terminal make sure you are in the frontend application folder and run the following command:

    yarn add cookieparser

now create the following file:

    /frontend/store/index.js

Copy and paste the following code in the file:

```js
import cookieparser from 'cookieparser'

export const actions = {  
  nuxtServerInit({ commit }, { req }) {
    let user = null
    if (req && req.headers && req.headers.cookie) {
      const parsed = cookieparser.parse(req.headers.cookie)
      user = (parsed.user && JSON.parse(parsed.user)) || null
    }

    commit('auth/setUser', user)
  }
}
```

Now lets create a login page so that users can login.

Create the following file:

    /frontend/pages/auth/signin.vue

Copy and paste in the following code:

```html
<template>
  <div>
    <div class="uk-child-width-1-3@m uk-grid">
      <div>
        <div class="uk-card uk-card-small uk-card-body"></div>
      </div>
      <div>
        <div class="uk-card uk-card-default uk-card-large uk-card-body">
          <form @submit.stop.prevent="handleSubmit">
            <fieldset class="uk-fieldset">
              <legend class="uk-legend">Sign in</legend>

              <div class="uk-margin">
                <label class="uk-form-label" for="form-stacked-text">Email</label>
                <input class="uk-input" v-model="email" type="email" placeholder="email" />
              </div>

              <div class="uk-margin">
                <label class="uk-form-label" for="form-stacked-text">Password</label>
                <input class="uk-input" v-model="password" type="password" placeholder="password" />
              </div>

              <div class="uk-margin">
                <button
                  class="uk-button uk-button-primary uk-width-1-1"
                  :disabled="loading"
                  type="submit"
                >Submit</button>
              </div>

              <div class="uk-margin">
                <p>
                  No account?
                  <router-link :to="{ name: 'auth-register'}">Register</router-link>
                </p>
              </div>
            </fieldset>
          </form>
        </div>
      </div>
      <div>
        <div class="uk-card uk-card-small uk-card-body"></div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapMutations } from "vuex";
import strapi from "~/store/Strapi";

export default {
  data() {
    return {
      email: "",
      password: "",
      loading: false
    };
  },
  methods: {
    async handleSubmit() {
      try {
        this.loading = true;
        const response = await strapi.login(this.email, this.password);
        this.loading = false;
        this.setUser(response.user);
        this.$router.push("/");
      } catch (err) {
        this.loading = false;
        alert(err.message || "An error occurred.");
      }
    },
    // call the auth code
    ...mapMutations({
      setUser: "auth/setUser"
    })
  }
};
</script>
```

Now run the frontend application and navigate around the application. In a terminal run:

``
  yarn dev
``
