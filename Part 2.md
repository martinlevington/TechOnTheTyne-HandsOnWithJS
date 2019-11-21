# Part 2 - Buuilding the UI Framework and Core Layouts

## Install UIkit

Our frontend application is going to use the UIkit front-end framework.  UIkit is a lightweight and modular framework for developing fast and powerful web interfaces.

In your terminal/console  make sure that you are in the correct directory. Then add the uikit package to your the frontend application using the following command.

    yarn add uikit  

Now you need to add the  UIkit to your Nuxt.js application (frontend).  We are going to do this by creating a plugin.

Create a new file in the directory:

    ./frontend/plugins/uikit.js

Now copy/paste the following code into uikit.js:
```js
    import Vue from 'vue'

    import UIkit from 'uikit/dist/js/uikit-core'  
    import Icons from 'uikit/dist/js/uikit-icons'

    UIkit.use(Icons)  
    UIkit.container = '#__nuxt'

    Vue.prototype.$uikit = UIkit 
```

Plugins need to be added to the 'nuxt.config.js' file and we will also add in the uikit css files here too.

Add the following code in your 'nuxt.config.js', replacing the existing `css: []` block

```
...
    css: [  
      "uikit/dist/css/uikit.min.css",
    ],
    /*
    ** Plugins to load before mounting the App
    */
    plugins: [  
      { 
        src: '~/plugins/uikit.js', ssr: false 
      }
    ],
...
```
## Page Layout

We will create a common layout for all of our pages. This layout  (layouts/default.vue) component will be the root of all the pages.  Inside this component is the  '\<nuxt />' tag with is the  application entry point.

Open the file called layout.vue

    /frontend/layouts/default.vue

Now copy and paste in the following code:
```html
    <template>  
        <div>
        <Header />
            <div class="uk-section uk-section-default">
                <div class="uk-container uk-container-large">
                    <nuxt />
                </div>
            </div>
        </div>
    </template>

    <script>  

    // Import your the Header component
    import Header from '~/components/Header.vue'  

    export default {  
        components: {
            Header
        }
    }
    </script>
```

## Design Assets

Out design uses a number fo common images. Copy the files located in the static directory/folder and save them into the frontend applications 'static' folder

The static assets can be found here:

    https://github.com/martinlevington/TechOnTheTyne-HandsOnWithJS




## Page Header

Now we will create a header which will be common across all of our pages.

Create a Header.vue component in components directory.

    i.e. /frontend/components/Header.vue

Paste in the following code:
```html
<template>
  <client-only>
    <nav class="uk-navbar-container" uk-navbar>
      <div class="uk-navbar-left">
        <ul class="uk-navbar-nav">
          <li class="uk-active">
            <router-link tag="a" class="navbar-brand" to="/" exact>
              <img src="/TechOnTheTyneSquare.png" alt="Tech on the Tyne" width="50" height="50" /> &nbsp; 
            </router-link>
          </li>
          <li>
            <router-link tag="a" class="navbar-brand" to="/takeaways" exact>Takeaways</router-link>
          </li>
        </ul>
      </div>

      <div class="uk-navbar-right">
      </div>
    </nav>
  </client-only>
</template>
```

## Home Page

Our application has the default starter home page. Lets update it, tidy up the page and make it out own.

The home page is located at:

    /frontend/pages/index.vue

Open the existing homepage and remove all of its content. Copy and paste the following code into the index.vue file:

```html
<template>
  <client-only>
    <nav class="uk-navbar-container" uk-navbar>
      <div class="uk-navbar-left">
        <ul class="uk-navbar-nav">
          <li class="uk-active">
            <router-link tag="a" class="navbar-brand" to="/" exact>
              <img src="/TechOnTheTyneSquare.png" alt="Tech on the Tyne" width="50" height="50" /> &nbsp; 
            </router-link>
          </li>
          <li>
            <router-link tag="a" class="navbar-brand" to="/takeaways" exact>Takeaways</router-link>
          </li>
        </ul>
      </div>

      <div class="uk-navbar-right">
      </div>
    </nav>
  </client-only>
</template>
```
Lets run our application agin to make sure that everything works.

In a terminal/console run the following command:

    yarn dev

Then visit :

    http://localhost:3000
