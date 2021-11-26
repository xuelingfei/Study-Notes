# Vuex 使用(v4.x)

<!-- markdown="1" is required for GitHub Pages to render the TOC properly. -->
<details markdown="1">
<summary>目录</summary>

- [在带命名空间的模块内访问全局内容](#在带命名空间的模块内访问全局内容)
- [在 Options API 中访问 store](#在-Options-API-中访问-store)
- [在 Componsition API 中访问 store](#在-Componsition-API-中访问-store)
- [结合 vuex-persist 实现持久化存储](#结合-vuex-persist-实现持久化存储)
- [全局示例](#全局示例)

</details>

## 在带命名空间的模块内访问全局内容

1. state 和 getter  
   如果你希望使用全局 state 和 getter, rootState 和 rootGetters 会作为第三和第四参数传入 getter, 也会通过 context 对象的属性传入 action。
2. action 和 mutation  
   若需要在全局命名空间内分发 action 或提交 mutation, 将 { root: true } 作为第三参数传给 dispatch 或 commit 即可。
3. 示例  
   nested.js

   ```js
   export default {
     namespaced: true,
     state: {
       shippingFee: 0,
     },
     getters: {
       // 在这个模块的 getter 中，`getters` 被局部化了
       // 你可以使用 getter 的第三、第四个参数来调用 `rootState` 和 `rootGetters`
       totalShippingFee: (state, getters, rootState) => {
         if (rootState.cart.items.count < 10) {
           return state.shippingFee
         } else {
           return 0
         }
       },
       cartTotalAmount: (state, getters, rootState, rootGetters) => {
         return getters.totalShippingFee + rootGetters.cart.cartTotalPrice
       },
     },
     mutations: {
       someMutation(state, payload) { ... }
     },
     actions: {
       // 在这个模块中， dispatch 和 commit 也被局部化了
       // 他们可以接受 `root` 属性以访问根 dispatch 或 commit
       someAction ({ dispatch, commit, getters, rootGetters }, product) {
         console.log(getters.shippingFee)
         console.log(rootGetters.cart.cartTotalPrice)

         commit('someMutation')
         commit('cart/pushProductToCart', product, { root: true })

         dispatch('someOtherAction')
         dispatch('cart/addProductToCart', product, { root: true })
       },
       someOtherAction (ctx, payload) { ... }
     }
   }
   ```

## 在 Options API 中访问 store

- 通过属性访问

  ```js
  computed: {
    count () {
      return this.$store.state.count
    },
    totalCount () {
      return this.$store.getters.totalCount
    },
    // 访问模块
    username () {
      return this.$store.state.user.name
    },
  }
  ```

- 通过辅助函数访问

  1. 通过使用 `createNamespacedHelpers` 访问单个模块中的属性

     ```js
     import { createNamespacedHelpers } from "vuex"
     const { mapState, mapGetters, mapMutations, mapActions } =
       createNamespacedHelpers("cart")

     export default {
       computed: {
         ...mapState(["items", "checkoutStatus"]),
         ...mapGetters(["cartProducts", "cartTotalPrice"]),
       },
       methods: {
         ...mapMutations(["pushProductToCart", "incrementItemQuantity"]),
         ...mapActions(["checkout", "addProductToCart"]),
       },
     }
     ```

  2. 访问单个或多个模块中的属性

     ```js
     import { mapState, mapGetters, mapMutations, mapActions } from "vuex"

     export default {
       computed: {
         ...mapState("user", ["name", "avatar"]),
         ...mapState("cart", ["items", "checkoutStatus"]),
         ...mapGetters("cart", ["cartProducts", "cartTotalPrice"]),
       },
       methods: {
         ...mapMutations("cart", [
           "pushProductToCart",
           "incrementItemQuantity",
         ]),
         ...mapActions("cart", ["checkout", "addProductToCart"]),
       },
     }
     ```

  3. 如果需要访问更深层的属性，或者给属性重命名，可参考如下示例

     ```js
     import { mapState, mapGetters } from "vuex"

     export default {
       computed: mapState("user", {
         // 传字符串参数 'name' 等同于 `state => state.name`
         name: "name",
         // 箭头函数可使代码更简练
         imgPath: (state) => state.avatar.imgPath,
         // 为了能够使用 `this` 获取局部状态，必须使用常规函数
         fullPath(state) {
           return this.host + state.avatar.imgPath
         },
       }),
       // 或
       // computed: mapState({
       //   name: state => state.user.name,
       //   imgPath: state => state.user.avatar.imgPath,
       //   fullPath(state) {
       //     return this.host + state.user.avatar.imgPath
       //   }
       // }),
     }
     ```

     或

     ```js
     import { mapState, mapGetters } from "vuex"

     export default {
       computed: {
         ...mapState("user", { avatar: (state) => state.avatar.imgPath }),
         // 或 ...mapState({ avatar: (state) => state.user.avatar.imgPath }),
         ...mapGetters("cart", { totalPrice: "cartTotalPrice" }),
       },
     }
     ```

## 在 Componsition API 中访问 store

- 示例

  ```js
  import { useStore } from "vuex"

  export default {
    setup() {
      const store = useStore()
      // 在 computed 函数中访问 state
      const checkoutStatus = computed(() => store.state.cart.checkoutStatus)
      // 在 computed 函数中访问 getter
      const products = computed(() => store.getters["cart/cartProducts"])
      const total = computed(() => store.getters["cart/cartTotalPrice"])
      // 使用 mutation
      const checkout = (products) =>
        store.commit("cart/pushProductToCart", products)
      // 使用 action
      const checkout = (products) => store.dispatch("cart/checkout", products)

      return {
        checkoutStatus,
        products,
        total,
        checkout,
      }
    },
  }
  ```

## 结合 vuex-persist 实现持久化存储

1. 背景：  
   Vuex 的状态存储并不能持久化，也就是说只要一刷新页面，存储在 store 中的数据就丢失了。而 vuex-persist 就是为 Vuex 持久化存储而生的一个插件，它能够将应用程序的状态保存到持久存储中，例如 Cookies 或 localStorage。
2. 安装：  
   `npm install --save vuex-persist` 或 `yarn add vuex-persist`
3. 用法：

   ```js
   import VuexPersistence from "vuex-persist"

   const vuexLocal = new VuexPersistence({
     storage: window.localStorage,
   })
   ```

## 全局示例

1. 目录结构

   ```sh
   store
   ├── index.js          # 组装模块并导出 store 的地方
   ├── actions.js        # 根级别的 action
   ├── mutations.js      # 根级别的 mutation
   └── modules
       ├── user.js       # 用户模块
       ├── cart.js       # 购物车模块
       └── products.js   # 产品模块
   ```

2. index.js

   ```js
   import { createStore, createLogger } from "vuex"
   import VuexPersistence from "vuex-persist"
   import user from "./modules/user"
   import cart from "./modules/cart"
   import products from "./modules/products"

   const debug = process.env.NODE_ENV !== "production"

   // 结合 vuex-persist 实现持久化存储
   const vuexLocal = new VuexPersistence({
     reducer: (state) => ({
       user: state.user,
       cart: state.cart,
       products: state.products,
     }),
     storage: window.localStorage,
   })

   export default createStore({
     state: {},
     mutations: {},
     actions: {},
     modules: {
       user,
       cart,
       products,
     },
     // 严格模式，不要在发布环境下启用
     strict: debug,
     // Vuex 自带一个日志插件用于一般的调试, logger 插件会生成状态快照。
     // 生成状态快照的插件应该只在开发阶段使用。
     plugins: debug ? [vuexLocal.plugin, createLogger()] : [vuexLocal.plugin],
   })
   ```

3. user.js

   ```js
   export default {
     namespaced: true,
     state: {
       name: "",
       avatar: {}, // { imgName: "", imgPath: "" }
     },
     mutations: {
       SET_USER(state, payload) {
         Object.assign(state, payload)
         state.isLogin = true
       },
       // 移除用户信息
       REMOVE_USER(state) {
         state.name = ""
         state.avatar = {}
       },
     },
   }
   ```

4. cart.js

   ```js
   import shop from "../../api/shop"
   import nested from "./nested"

   // initial state
   // shape: [{ id, quantity }]
   const state = () => ({
     items: [],
     checkoutStatus: null,
   })

   // getters
   const getters = {
     cartProducts: (state, getters, rootState) => {
       return state.items.map(({ id, quantity }) => {
         const product = rootState.products.all.find(
           (product) => product.id === id
         )
         return {
           id: product.id,
           title: product.title,
           price: product.price,
           quantity,
         }
       })
     },

     cartTotalPrice: (state, getters) => {
       return getters.cartProducts.reduce((total, product) => {
         return total + product.price * product.quantity
       }, 0)
     },
   }

   // mutations
   const mutations = {
     pushProductToCart(state, { id }) {
       state.items.push({
         id,
         quantity: 1,
       })
     },

     incrementItemQuantity(state, { id }) {
       const cartItem = state.items.find((item) => item.id === id)
       cartItem.quantity++
     },

     setCartItems(state, { items }) {
       state.items = items
     },

     setCheckoutStatus(state, status) {
       state.checkoutStatus = status
     },
   }

   // actions
   const actions = {
     async checkout({ commit, state }, products) {
       const savedCartItems = [...state.items]
       commit("setCheckoutStatus", null)
       // empty cart
       commit("setCartItems", { items: [] })
       try {
         await shop.buyProducts(products)
         commit("setCheckoutStatus", "successful")
       } catch (e) {
         console.error(e)
         commit("setCheckoutStatus", "failed")
         // rollback to the cart saved before sending the request
         commit("setCartItems", { items: savedCartItems })
       }
     },

     addProductToCart({ state, commit }, product) {
       commit("setCheckoutStatus", null)
       if (product.inventory > 0) {
         const cartItem = state.items.find((item) => item.id === product.id)
         if (!cartItem) {
           commit("pushProductToCart", { id: product.id })
         } else {
           commit("incrementItemQuantity", cartItem)
         }
         // remove 1 item from stock
         commit(
           "product/decrementProductInventory",
           { id: product.id },
           { root: true }
         )
       }
     },
   }

   export default {
     namespaced: true,
     state,
     getters,
     mutations,
     actions,
     // 模块可以继续嵌套子模块
     modules: {
       nested,
     },
   }
   ```

5. product.js

   ```js
   import shop from "../../api/shop"

   // initial state
   const state = () => ({
     all: [],
   })

   // mutations
   const mutations = {
     setProducts(state, products) {
       state.all = products
     },

     decrementProductInventory(state, { id }) {
       const product = state.all.find((product) => product.id === id)
       product.inventory--
     },
   }

   // actions
   const actions = {
     async getAllProducts({ commit }) {
       const products = await shop.getProducts()
       commit("setProducts", products)
     },
   }

   export default {
     namespaced: true,
     state,
     actions,
     mutations,
   }
   ```

6. nested.js

   ```js
   export default {
     namespaced: true,
     state: () => ({}),
   }
   ```
