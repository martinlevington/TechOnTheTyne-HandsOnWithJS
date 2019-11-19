# Part 5: Displaying the Takeways in the frontend Application

To display the 'takeaways' data in our website we will need to query our GraphQL endpoint from the frontend application. To do this we will install a client library called nuxtjs/appollo. Go back to uou terminal and make sure you are in the frontend application folder. Now run the following command to install the client library.

```
    yarn add @nuxtjs/apollo graphql
```

The newly installed module will need to be added to the 'nuxt.config.js' file.

Open the nuxt.config.js file and add the following lines to it:

```js
...
    modules: [  
        '@nuxtjs/apollo',
    ],
    apollo: {  
        clientConfigs: {
            default: {
            httpEndpoint: 'http://localhost:1337/graphql'
            }
        }
    },
...
```
Now lets create a page to display our 'takeaways' venues on.

Create the following folder and file.
    
    /pages/takeaways/index.vue


```html
<template>
  <div>
    <!-- The search input to filters takeaways -->
    <form class="uk-search uk-search-default uk-search-large uk-align-center uk-margin">
      <span uk-search-icon></span>
      <input class="uk-search-input" v-model="query" type="search" placeholder="Search..." />
    </form>
    <!-- UIKit Display Cards -->
    <div
      class="uk-card uk-card-default uk-grid-collapse uk-child-width-1-2@m uk-margin"
      v-for="takeaway in filteredList"
      v-bind:key="takeaway.id"
      uk-grid
    >
      <div class="uk-card-media-left uk-cover-container">
        <img :src="'http://localhost:1337/' + takeaway.image.url" alt uk-cover />
        <canvas width="600" height="400"></canvas>
      </div>
      <div>
        <div class="uk-card-body">
          <h3 class="uk-card-title">{{ takeaway.name }}</h3>
          <p>{{ takeaway.description }}</p>
          <!-- Generate a link to the takeaway using router-link -->
          <router-link
            :to="{ name: 'takeaways-id', params: { id: takeaway.id }}"
            tag="a"
            class="uk-button uk-button-primary"
          >See Menu</router-link>
        </div>
      </div>
    </div>
    <!--  If no takeaways have been found -->
    <div class="uk-container uk-container-center uk-text-center" v-if="filteredList.length == 0">
      <p>No records found</p>
    </div>
  </div>
</template>

<script>
// Import the takeaways query
import takeawaysQuery from '~/apollo/queries/takeaway/takeaways';

export default {
  data() {
    return {
      // Initialize variables
      takeaways: [],
      query: ""
    };
  },
  apollo: {
    takeaways: {
      prefetch: true,
      query: takeawaysQuery
    }
  },
  computed: {
    // Search system
    filteredList() {
      return this.takeaways.filter(takeaway => {
        return takeaway.name.toLowerCase().includes(this.query.toLowerCase());
      });
    }
  }
};
</script>
```

Now lets add or GraphQL query, create the following folder and file in your frontend folder:

```
    /apollo/queries/takeaway/takeaways.gql
```

and copy/paste the following code:

```graphql
    query Takeaways {  
    takeaways {
        id
        name
        description
        delivery_time
        image {
            url
            }
        }
    }
```

Now lets run our frontend application. Run the following command:

```
    yarn dev
```
