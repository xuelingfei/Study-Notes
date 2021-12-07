# Vue Router 使用(v4.x)

<!-- markdown="1" is required for GitHub Pages to render the TOC properly. -->
<details markdown="1">
<summary>目录</summary>

- [router 和 route](#router-和-route)
- [router-link 和 router-view](#router-link-和-router-view)
- [重定向和别名](#重定向和别名)
- [命名视图](#命名视图)
- [导航](#导航)
- [路由匹配](#路由匹配)
- [在 Options API 中访问路由](#在-options-api-中访问路由)
- [在 Componsition API 中访问路由](#在-componsition-api-中访问路由)
- [路由懒加载](#路由懒加载)
- [全局示例](#全局示例)
- [Vue Router 官网](#vue-router-官网)

</details>

## router 和 route

1. router - 全局路由实例  
   router 是 Vue Router 构造方法的实例。常用的路由实例方法有 `router.push()`, `router.replace()`, `router.go()` 等。

2. route - 路由信息对象  
   一个路由对象 (route object) 表示当前激活的路由的状态信息，包含了当前 url 解析得到的信息，还有 url 匹配到的路由记录 (route records)。  
   路由对象是不可变 (immutable) 的，每次成功的导航后都会产生一个新的对象。

   - `route.name` - 当前路由的名称，如果有的话。
   - `route.meta` - 当前路由的路由元信息。
   - `route.path` - 字符串，对应当前路由的路径，总是解析为绝对路径，如 "/foo/bar"。
   - `route.params` - 一个 key/value 对象，包含了动态片段和全匹配片段，如果没有路由参数，就是一个空对象。
   - `route.query` - 一个 key/value 对象，表示 url 查询参数。例如，对于路径 "/foo?user=1"，则有 `route.query.user == 1`，如果没有查询参数，则是个空对象。
   - `route.hash` - 当前路由的 hash 值（带 #） ，如果没有 hash 值，则为空字符串。
   - `route.fullPath` - 字符串，完成解析后的 url, 包含查询参数和 hash 的完整路径。
   - `route.redirectedFrom` - 如果存在重定向，即为重定向来源的路由的名字。
   - `route.matched` - 一个数组 (Array\<RouteRecord\>)，包含当前路由的所有嵌套路径片段的路由记录 。路由记录就是 routes 配置数组中的对象副本（还有在 children 数组）。
     ```js
     const router = new VueRouter({
       routes: [
         // 下面的对象就是路由记录
         {
           path: "/foo",
           component: Foo,
           children: [
             // 这也是个路由记录
             { path: "bar", component: Bar },
           ],
         },
       ],
     })
     ```
     当 url 为 "/foo/bar"，`route.matched` 将会是一个包含从上到下的所有对象 (副本)。

## router-link 和 router-view

1. router-link  
   使用自定义组件 router-link 来创建链接而没有使用常规的 a 标签，这使得 Vue Router 可以在不重新加载页面的情况下更改 url, 处理 url 的生成以及编码。
2. router-view  
   router-view 将显示与 url 对应的组件。你可以把它放在任何地方，以适应你的布局。
3. 示例

   ```html
   <script src="https://unpkg.com/vue@3"></script>
   <script src="https://unpkg.com/vue-router@4"></script>

   <div id="app">
     <h1>Hello App!</h1>
     <p>
       <!--使用 router-link 组件进行导航 -->
       <!--通过传递 `to` 来指定链接 -->
       <!--`<router-link>` 将呈现一个带有正确 `href` 属性的 `<a>` 标签-->
       <router-link to="/">Go to Home</router-link>
       <router-link to="/about">Go to About</router-link>
     </p>
     <!-- 路由出口 -->
     <!-- 路由匹配到的组件将渲染在这里 -->
     <router-view></router-view>
   </div>
   ```

## 重定向和别名

1. 重定向

   ```js
   // 重定向也是通过 routes 配置来完成
   const routes = [{ path: "/home", redirect: "/" }]
   // 重定向的目标也可以是一个命名的路由
   const routes = [{ path: "/home", redirect: { name: "homepage" } }]
   // 甚至是一个方法，动态返回重定向目标
   const routes = [
     {
       // /search/screens -> /search?q=screens
       path: "/search/:searchText",
       redirect: (to) => {
         // 方法接收目标路由作为参数
         // return 重定向的字符串路径/路径对象
         return { path: "/search", query: { q: to.params.searchText } }
       },
     },
     {
       path: "/search",
       // ...
     },
   ]
   ```

   在写 `redirect` 的时候，可以省略 `component` 配置，因为它从来没有被直接访问过，所以没有组件要渲染。唯一的例外是**嵌套路由**：如果一个路由记录有 `children` 和 `redirect` 属性，它也应该有 `component` 属性。

2. 别名  
   通过别名，可以将 UI 结构映射到一个任意的 URL，而不受配置的嵌套结构的限制。
   ```js
   // 将 `/home` 别名为 `/`，意味着当用户访问 `/` 时，URL 仍然是 `/`，但会被匹配为用户正在访问 `/home`。
   const routes = [{ path: "/home", component: Homepage, alias: "/" }]
   ```
   使别名以 `/` 开头，以使嵌套路径中的路径成为绝对路径。也可以将两者结合起来，用一个数组提供多个别名。
   ```js
   const routes = [
     {
       path: "/users",
       component: UsersLayout,
       children: [
         // 为这 3 个 URL 呈现 UserList
         // - /users
         // - /users/list
         // - /people
         { path: "", component: UserList, alias: ["/people", "list", ""] },
       ],
     },
   ]
   ```
   如果你的路由有参数，请确保在任何绝对别名中包含它们：
   ```js
   const routes = [
     {
       path: "/users/:id",
       component: UsersByIdLayout,
       children: [
         // 为这 3 个 URL 呈现 UserDetails
         // - /users/24
         // - /users/24/profile
         // - /24
         { path: "profile", component: UserDetails, alias: ["/:id", ""] },
       ],
     },
   ]
   ```

## 命名视图

有时候想同级展示多个视图，而不是嵌套展示，例如创建一个布局，有 sidebar (侧导航) 和 main (主内容) 两个视图，这个时候命名视图就派上用场了。你可以在界面中拥有多个单独命名的视图，而不是只有一个单独的出口。如果 router-view 没有设置名字，那么默认为 default。

```html
<router-view class="view header" name="Header"></router-view>
<router-view class="view sidebar" name="Sidebar"></router-view>
<router-view class="view main-content"></router-view>
```

一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件。确保正确使用 components 配置 (带上 s)：

```js
const router = createRouter({
  history: createWebHashHistory(),
  routes: [
    {
      path: "/",
      components: {
        default: Home,
        // 与 `<router-view>` 上的 `name` 属性匹配
        header: Header,
        sidebar: Sidebar,
      },
      // 对于有命名视图的路由，你必须为每个命名视图定义 props 配置
      props: { default: true, header: false, sidebar: false },
    },
  ],
})
```

## 导航

1. push - 导航到不同位置  
   这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，会回到之前的 url, 类似于 `window.history.pushState`。

   |          声明式           |       编程式       |
   | :-----------------------: | :----------------: |
   | `<router-link :to="...">` | `router.push(...)` |

   示例

   ```js
   // 字符串路径
   router.push("/users/eduardo")
   // 带有路径的对象
   router.push({ path: "/users/eduardo" })
   // 命名的路由，并加上参数，让路由建立 url
   router.push({ name: "user", params: { username: "eduardo" } })
   // 带查询参数，结果是 /register?plan=private
   router.push({ path: "/register", query: { plan: "private" } })
   // 带 hash，结果是 /about#team
   router.push({ path: "/about", hash: "#team" })

   const username = "eduardo"
   // 我们可以手动建立 url，但我们必须自己处理编码
   router.push(`/user/${username}`) // -> /user/eduardo
   // 同样
   router.push({ path: `/user/${username}` }) // -> /user/eduardo
   // 如果可能的话，使用 `name` 和 `params` 从自动 URL 编码中获益
   router.push({ name: "user", params: { username } }) // -> /user/eduardo
   // `params` 不能与 `path` 一起使用, params 会被忽略，上述例子中的 query 并不属于这种情况
   router.push({ path: "/user", params: { username } }) // -> /user
   ```

2. replace - 替换当前位置  
   与 `router.push()` 不同的是，它在导航时不会向 history 添加新记录，正如它的名字所暗示的那样——它取代了当前的条目，类似于 `window.history.replaceState()`。

   |              声明式               |        编程式         |
   | :-------------------------------: | :-------------------: |
   | `<router-link :to="..." replace>` | `router.replace(...)` |

   示例

   ```js
   router.push({ path: "/home", replace: true })
   // 相当于
   router.replace({ path: "/home" })
   ```

3. go - 横跨历史  
   该方法采用一个整数作为参数，表示在历史堆栈中前进或后退多少步，类似于 `window.history.go(n)`。

   示例

   ```js
   // 向前移动一条记录，与 router.forward() 相同
   router.go(1)

   // 返回一条记录，与router.back() 相同
   router.go(-1)

   // 前进 3 条记录
   router.go(3)

   // 如果没有那么多记录，静默失败
   router.go(-100)
   router.go(100)
   ```

## 路由匹配

1. 带参数的动态路由匹配  
   很多时候，我们需要将给定匹配模式的路由映射到同一个组件。例如，我们可能有一个 User 组件，它应该对所有用户进行渲染，但用户 ID 不同。在 Vue Router 中，我们可以在路径中使用一个动态段来实现，我们称之为路径参数。  
   路径参数用冒号 `:` 表示。当一个路由被匹配时，它的 `params` 的值将在每个组件中以 `this.$route.params` 的形式暴露出来。

   ```js
   const User = {
     template: "<div>User {{ $route.params.id }}</div>",
   }

   // 这些都会传递给 `createRouter`
   const routes = [
     // 动态段以冒号开始
     { path: "/users/:id", component: User },
   ]
   ```

   常规参数只匹配 URL 片段之间的字符，用 `/` 分隔。你可以在同一个路由中设置有多个路径参数，它们会映射到 `$route.params` 上的相应字段。例如：

   | 匹配模式                         | 匹配路径                 | $route.params                          |
   | -------------------------------- | ------------------------ | -------------------------------------- |
   | `/users/:username `              | /users/eduardo           | { username: 'eduardo' }                |
   | `/users/:username/posts/:postId` | /users/eduardo/posts/123 | { username: 'eduardo', postId: '123' } |

   如果我们想匹配任意路径，我们可以使用自定义的路径参数正则表达式，在路径参数后面的括号中加入正则表达式。

   ```js
   const routes = [
     // 将匹配所有内容并将其放在 `$route.params.pathMatch` 下
     { path: "/:pathMatch(.*)*", name: "NotFound", component: NotFound },
     // 将匹配以 `/user-` 开头的所有内容，并将其放在 `$route.params.afterUser` 下
     { path: "/user-:afterUser(.*)", component: UserGeneric },
   ]
   ```

2. 相应路由参数的变化
   使用带有参数的路由时需要注意的是，当用户从 /users/johnny 导航到 /users/jolyne 时，**相同的组件实例将被重复使用**。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。不过，**这也意味着组件的生命周期钩子不会被调用**。

   要对同一个组件中参数的变化做出响应的话，你可以简单地 watch `$route` 对象上的任意属性，在这个场景中，就是 `$route.params` ：

   ```js
   const User = {
     template: "...",
     created() {
       this.$watch(
         () => this.$route.params,
         (toParams, previousParams) => {
           // 对路由变化做出响应...
         }
       )
     },
   }
   ```

   或者，使用 beforeRouteUpdate 导航守卫，它也可以取消导航：

   ```js
   const User = {
     template: "...",
     async beforeRouteUpdate(to, from) {
       // 对路由变化做出响应...
       this.userData = await fetchUser(to.params.id)
     },
   }
   ```

## 在 Options API 中访问路由

通过在 Vue 根实例的 router 配置传入 router 实例，下面这些属性成员会被注入到每个子组件。

- this.$router  
  router 实例。
- this.$route  
  当前激活的路由信息对象。这个属性是只读的，里面的属性是不可变 (immutable) 的，不过你可以监测 (watch) 它的变化。

## 在 Componsition API 中访问路由

在 setup 中访问路由和当前路由，因为在 setup 里面没有访问 this，所以我们不能再直接访问 `this.$router` 或 `this.$route`。作为替代，我们使用 useRouter 和 useRoute 函数。
但请注意，在模板中我们仍然可以访问 `this.$router` 或 `this.$route`，所以不需要在 setup 中返回 router 或 route。

```js
import { useRouter, useRoute } from "vue-router"

export default {
  setup() {
    const router = useRouter()
    const route = useRoute()

    function pushWithQuery(query) {
      router.push({
        name: "search",
        query: { ...route.query },
      })
    }

    const userData = ref()

    // 当参数更改时获取用户信息
    watch(
      () => route.params,
      async (newParams) => {
        userData.value = await fetchUser(newParams.id)
      }
    )

    return { pushWithQuery, userData }
  },
}
```

route 对象是一个响应式对象，所以它的任何属性都可以被监听，但应该避免监听整个 route 对象。

## 路由懒加载

1. 动态导入  
   Vue Router 支持开箱即用的动态导入，这意味着你可以用动态导入代替静态导入：

   ```js
   // 将
   // import UserDetails from './views/UserDetails'
   // 替换成
   const UserDetails = () => import("./views/UserDetails")

   const router = createRouter({
     // ...
     routes: [{ path: "/users/:id", component: UserDetails }],
   })
   ```

   component (和 components) 配置接收一个返回 Promise 组件的函数，Vue Router 只会在第一次进入页面时才会获取这个函数，然后使用缓存数据。一般来说，对所有的路由都使用动态导入是个好主意。

2. 把组件按组分块  
   有时候我们想把某个路由下的所有组件都打包在同个异步块 (chunk) 中。只需要使用命名 chunk，一个特殊的注释语法来提供 chunk name (需要 Webpack > 2.4)：
   ```js
   const UserDetails = () =>
     import(/* webpackChunkName: "group-user" */ "./UserDetails.vue")
   const UserDashboard = () =>
     import(/* webpackChunkName: "group-user" */ "./UserDashboard.vue")
   const UserProfileEdit = () =>
     import(/* webpackChunkName: "group-user" */ "./UserProfileEdit.vue")
   ```
   webpack 会将任何一个异步模块与相同的块名称组合到相同的异步块中。

## 全局示例

1. 目录结构

   ```sh
   src
   ├── main.js
   └── router
       ├── index.js
       └── modules
           ├── home.js
           └── cart.js
   ```

2. main.js

   ```js
   import { createApp } from "vue"
   import App from "./App.vue"
   import router from "./router"

   const app = createApp(App)
   app.use(router)

   ...

   app.mount("#app")
   ```

3. index.js

   ```js
   import { createRouter, createWebHistory } from "vue-router"
   import homeRouter from "./module/home"
   import cartRouter from "./module/cart"

   // 每个路由都需要映射到一个组件
   const routes = [
     { path: "/", redirect: "/home" },
     {
       path: "/",
       name: "MainLayout",
       component: () =>
         import(
           /* webpackChunkName: "MainLayout" */ "@/layout/MainLayout/index.vue"
         ),
       children: [...homeRouter],
     },
     ...cartRouter,
   ]

   // 创建路由实例并传递 `routes` 配置
   const router = createRouter({
     // history 模式
     history: createWebHistory(process.env.BASE_URL),
     // hash 模式
     // history: createWebHashHistory(),
     routes,
   })

   export default router
   ```

4. home.js

   ```js
   const homeRouter = [
     {
       path: "/home",
       name: "Home",
       component: () =>
         import(/* webpackChunkName: "Home" */ "@/views/home/index.vue"),
       meta: { title: "首页" },
     },
   ]
   export default homeRouter
   ```

5. cart.js

   ```js
   export default []
   ```

## Vue Router 官网

官网地址：<https://next.router.vuejs.org>
