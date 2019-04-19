<template>
  <v-app dark>
    <v-navigation-drawer
      v-model="drawer"
      :mini-variant="miniVariant"
      :clipped="clipped"
      fixed
      app
    >
      <v-list class="pa-3 cart-container">
        <div class="pa-1 w-100 text-xs-center">
          Shopping cart
        </div>
        <hr />
        <drop
          class="drop list w-100 h-100 py-2"
          @drop="handleDrop(cart, ...arguments)"
        >
          <drag
            v-for="item in cart"
            :key="item"
            class="drag"
            :class="{ [item]: true }"
            :transfer-data="{ item: item, list: cart }"
          >
            {{ item }}
          </drag>
        </drop>
      </v-list>
    </v-navigation-drawer>
    <v-toolbar :clipped-left="clipped" fixed app>
      <v-btn icon @click="drawer = !drawer">
        <v-icon>shopping_cart</v-icon>
      </v-btn>
      <v-toolbar-title v-text="title" />
      <v-spacer />
      <v-btn icon @click.stop="rightDrawer = !rightDrawer">
        <v-icon>list</v-icon>
      </v-btn>
    </v-toolbar>
    <v-content>
      <v-container>
        <nuxt />
        <template>
          <div>
            <div v-for="(list, i) in lists" :key="i">
              <h2 class="w-100 dark-text pa-2">Category {{ i + 1 }}</h2>
              <drop class="drop list" @drop="handleDrop(list, ...arguments)">
                <drag
                  v-for="item in list"
                  :key="item"
                  class="drag"
                  :class="{ [item]: true }"
                  :transfer-data="{ item: item, list: list, example: 'lists' }"
                >
                  {{ item }}
                </drag>
              </drop>
            </div>
          </div>
        </template>
      </v-container>
    </v-content>
    <v-navigation-drawer v-model="rightDrawer" :right="right" fixed>
      <v-list>
        <v-list-tile
          v-for="(item, i) in items"
          :key="i"
          :to="item.to"
          router
          exact
        >
          <v-list-tile-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title v-text="item.title" />
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>
    <v-footer :fixed="fixed" app>
      <span>&copy; 2019</span>
    </v-footer>
  </v-app>
</template>

<script>
import { Drag, Drop } from 'vue-drag-drop'
export default {
  components: { Drag, Drop },
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'bubble_chart',
          title: 'Inspire',
          to: '/inspire'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'My E-commerce',
      lists: [
        ['Mobile 1', 'Mobile 2', 'Mobile 3'],
        ['Laptop 1', 'Laptop 2', 'Laptop 3']
      ],
      cart: []
    }
  },
  methods: {
    handleDrop(toList, data) {
      const fromList = data.list
      if (fromList) {
        toList.push(data.item)
        fromList.splice(fromList.indexOf(data.item), 1)
        toList.sort((a, b) => a > b)
      }
    }
  }
}
</script>

<style lang="css">
hr {
  border-color: #555 !important;
  border-style: solid !important;
}
.drag {
  display: inline-block;
  width: 100px;
  height: 100px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #444;
  margin: 10px
}
.drop {
  border-radius: 10px !important;
  background-color: #CCC;
  display: flex;
  vertical-align: top;
  padding: 15px;
  margin-bottom: 20px;
  width: auto;
  height: auto;
}
.v-navigation-drawer .drop {
  background-color: transparent !important;
  display: block !important;
  height: 90% !important
}

.v-navigation-drawer .drop .drag {
  background-color: #CCC !important;
  color: #444 !important;
  width: 100% !important;
  height: 30px !important;
}
.cart-container {
  height: 100% !important;
}
</style>
