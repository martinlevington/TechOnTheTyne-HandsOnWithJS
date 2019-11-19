# Part 6:  Creating Food Items for The Takeaways Menu in the CMS

Every takeaways has a menu that lists the various dishes that they sell. We will now create a new content called 'menuitem'.  Login to 'Strapi' CMS admin again:

    http://localhost:1337/admin/plugins/content-type-builder/models/permission&source=users-permissions

Click on 'Add a Content Type' enter 'menuitem' as the name as click save.

Use the CMS Create the followings fields for menuitem:
    
    - 'name' with type String
    - 'description' with type  Text
    - 'image' with type Media
    - 'price' with type Number and Number format decimal
    - 'takeaway' with type Relation: here we need to define that every 'menuitem' can be related to a 'takeaway'. To do so, create a one-to-many relation. In the right menu dropdown from Permission to 'Takeaway'. Click on the 'many to one' icon (Takeaway has many MenuItems)

Now click 'Done', then click 'Save'

We also need to allow access in the Roles & Permissions section:

**Public Permissions for MenuItem**

For the 'Public' role,  Navigate to the Roles & Permissions section of the admin plugin http://localhost:1337/admin/plugins/users-permissions

Click on the 'Public' role
Check the 'find' and 'findone' checkboxes of the 'Menuitem' section
Then click the 'Save' button

**Authenticated Permissions for MenuItem**

For the 'Authenticated' Navigate to the Roles & Permissions section of the admin plugin http://localhost:1337/admin/plugins/users-permissions

Click on the 'Authenticated' role
Check the 'find' and 'findone' checkboxes of the 'Menuitem' section
Then click the 'Save' button

## Add Menu Items

Now add some menu items in the Strapi admin interface. There are some images that you can use for this located here:

Note: make sure you link your menuitems to your takeaway.

```
  https://github.com/martinlevington/TechOnTheTyne-HandsOnWithJS/tree/master/takeaways
```


## Displaying Menu Items

We will now create a page for each Takeaway that displays the and over of the Takeaway and its menu.

Create a file called '_id.vue' in the following location in your frontend application directory:

```
    /frontend/pages/takeaways/_id.vue
```

Copy and paste in the following code:

```html
<template>  
<div>

  <a class="uk-button uk-button-primary uk-margin" @click="$router.go(-1)"><span uk-icon="arrow-left"></span> go back</a>

  <client-only>
  <div uk-grid>

      <!-- // Card  Left -->
      <div class="uk-width-1-3@m">
       <div v-for="item in takeaway.menuitems" v-bind:key="item.id" class="uk-margin">
            <div class="uk-card uk-card-default">
                <div class="uk-card-media-top">
                    <img :src="'http://localhost:1337/' + item.image.url" alt="" />
                </div>
                <div class="uk-card-body">
                    <h3 class="uk-card-title">{{ item.name }} <span class="uk-badge">{{ item.price }}€</span></h3>
                    <p>{{ item.description }}</p>
                </div>
                <div class="uk-card-footer">
                  <button class="uk-button uk-button-primary">Add to basket</button>
                </div>
            </div>
        </div>
      </div>

      <!-- // Card right -->
      <div class="uk-width-expand@m">
      </div>

  </div>

  </client-only>
</div>  
</template>

<script>  
import takeawayQuery from '~/apollo/queries/takeaway/takeaway'

export default {  
  data() {
    return {
      takeaway: Object
    }
  },
  apollo: {
    takeaway: {
      prefetch: true,
      query: takeawayQuery,
      variables () {
        return { id: this.$route.params.id }
      }
    }
  }
}
</script>
```

We will also need to create our Takeaway query:

Create a new file named 'takeaway.gql' located at:

```
    /frontend/apollo/queries/takeaway/takeaway.gql
```

Copy and paste the following code into this file:

```graphql
query Takeaway ($id: ID!) {  
  takeaway(id: $id) {
    id
    name
    menuitems {
      id
      name
      description
      price
      image {
        url
      }
    }
  }
}
```

Now run the frontend application and navigate around the application. In a terminal run:

``
  yarn dev
``
