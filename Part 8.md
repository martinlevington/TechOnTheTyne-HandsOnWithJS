# Part 8 - Shopping Basket

Now that we have our online store, with a selection of Takeaways serving delicious food lets enable users to add them to a shopping basket so that they can select the dishes that they wish to order.

We will create a shopping basket. First create the following file:

    ./frontend/store/basket.js

Now copy and paste in the following code:

```js
import Cookies from 'js-cookie'

export const state = () => ({  items: [] })

export const mutations = {  
  setItems(state, items) {
    state.items = items
  },
  add(state, item) {
    const record = state.items.find(i => i.id === item.id)

    if (!record) {
      state.items.push({
        quantity: 1,
        ...item
      })
    } else {
      record.quantity++
    }
    Cookies.set('basket', state.items)
  },
  remove(state, item) {
    const record = state.items.find(i => i.id === item.id)

    if (record.quantity > 1) {
      record.quantity--
    } else {
      const index = state.items.findIndex(i => i.id === item.id)
      state.items.splice(index, 1)
    }
    Cookies.set('basket', state.items)
  },
  emptyList(state) {
    state.items = []
    Cookies.set('basket', state.items)
  }
}

export const getters = {  
  items: state => {
    return state.items
  },
  price: state => {
    return state.items.reduce(
      (accumulator, item) => accumulator + item.price * item.quantity, 0)
  },
  numberOfItems: state => {
    return state.items.reduce(
      (accumulator, item) => accumulator + item.quantity, 0)
  }
}
```

We need a basket Component so that we can view the contents of our basket. We will create a component so that we can reuse it on multiple pages as required.

create the following file:

    /frontend/components/Basket.vue

Now copy and paste in the following code into the file:

```html
<template>
  <div class="uk-card uk-card-default uk-card-body uk-margin" uk-sticky="offset: 20; bottom: true">
    <img
      src="https://img.icons8.com/dotty/80/000000/shopping-basket.png"
      class="uk-align-right"
      height="100"
      width="100"
      alt
    />
    <div v-if="price > 0">
      <table class="uk-table uk-table-striped uk-table-small uk-table-responsive">
        <thead>
          <tr>
            <th>Name</th>
            <th>Price (unit)</th>
            <th>Quantity</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="item in selectedItems" v-bind:key="item.id">
            <td class="uk-width-1-2">{{ item.name }}</td>
            <td class="uk-table-shrink">£{{ item.price }}</td>
            <td class="uk-table-shrink">{{ item.quantity }}</td>
            <td>
              <a class="uk-margin-left">
                <span class="uk-badge" @click="addToBasket(item)">+</span>
              </a>
              <a>
                <span
                  class="uk-badge"
                  style="background: #f0506e;"
                  @click="removeFromBasket(item)"
                >-</span>
              </a>
            </td>
          </tr>
          <tr>
            <td colspan="4" class="uk-text-right">Basket Total £{{ price }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import { mapMutations } from "vuex";

export default {
  methods: {
    ...mapMutations({
      addToBasket: "basket/add",
      removeFromBasket: "basket/remove"
    })
  },
  computed: {
    id() {
      return this.$route.params.id;
    },
    selectedItems() {
      return this.$store.getters["basket/items"];
    },
    price() {
      return this.$store.getters["basket/price"];
    },
    numberOfItems() {
      return this.$store.getters["basket/numberOfItems"];
    }
  }
};
</script>
```



Now lets create a Basket page to view the contents of the basket.

Create the following file:

```
    /frontend/pages/store/basket.vue
```

Copy and paste the following code into the created file:

```html
<template>  
<div>

  <a class="uk-button uk-button-primary uk-margin" @click="$router.go(-1)"><span uk-icon="arrow-left"></span> go back</a>

  <client-only placeholder="Chargement...">

    <div uk-grid>
        <div class="uk-width-expand@m">
            <Basket />
        </div>
    </div>
  </client-only>


</div>  
</template>

<script>  
import Basket from '~/components/Basket.vue'  

export default {  
  components: {
    Basket
  },
  methods: {
    async handleSubmit() {
    
    }
  }
}
</script>
```


Now run the frontend application and navigate around the application. In a terminal run:

``
  yarn dev
``

Add some menu items to the basket, and click on the basket icon at the top to view the contents of your basket.