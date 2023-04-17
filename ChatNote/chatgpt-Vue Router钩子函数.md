**You:**

vue router有哪些钩子？

* * *

**ChatGPT:**

Vue Router 提供了以下导航钩子函数：

1.  `beforeEach`: 在路由跳转前被调用，可以用来做全局的导航守卫，常用于用户登录状态验证等操作。
    
2.  `beforeResolve`: 在 `beforeEach` 之后，路由跳转前被调用，可以用来做路由切换前的最后一步操作，如确认弹窗等。
    
3.  `afterEach`: 路由跳转完成后被调用，可以用来做页面滚动、页面访问统计等操作。
    
4.  `beforeEnter`: 路由配置中的钩子，用于对单个路由进行导航守卫。
    
5.  `beforeRouteUpdate`: 路由配置中的钩子，用于对单个路由进行导航守卫，但是当前路由组件被复用时不会触发 `beforeEnter` 钩子。
    
6.  `beforeRouteLeave`: 路由配置中的钩子，用于对单个路由进行导航守卫，当当前路由组件即将离开时被调用，可以用来做表单未提交确认弹窗等操作。
    

以上是 Vue Router 提供的常用导航钩子函数。除此之外，还有一些高级钩子函数可以用来进行更细粒度的控制，如 `scrollBehavior`、`beforeRouteEnter` 等。