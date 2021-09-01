1. Vue的生命周期

   ```
      var vm = new Vue({
           el: '#app',
           data: {
               message: 'hello world'
           },
           methods: {
               changeMsg () {
                   this.message = 'goodbye world'
               }
           },
           beforeCreate() {
               console.log('------初始化前------')
              
           },
           created () {
               console.log('------初始化完成------')
              
           },
           beforeMount () {
               console.log('------挂载前---------')
               
           },
           mounted () {
               console.log('------挂载完成---------')
              
           },
           beforeUpdate () {
               console.log('------更新前---------')
            
           },
           updated() {
               console.log('------更新后---------')
              
           }
       })
   _________________________________________________-
   
   beforeDestroy 
   Destroy
   ```

2. created和mounted的区别

   * created 在模板渲染成 html前调用，主要用来初始化数据 
   * mounted 在模板渲染成html后调用，通常是初始化页面完成后，再对HTM中的DOM节点进行操作 ，如echarts中，就必须得dom节点加载完后才能进行初始化配置

3. 对Vue中 keep-alive的理解和使用 

   > keep-alive 是Vue内置的一个组件，可以是被包含的组件保留状态，或避免重新渲染，也就是组件缓存

   被包含在<keep-alive> 中创建的组件，会新增两个生命周期的钩子， activated与 deactivated

    activated ：当 keep-alive 包含的组件再次渲染的时候触发

   deactivated ：当 keep-alive 包含的组件销毁的时候触发

4. Vue双向数据绑定原理

   > 其核心是 Object.defineProperty()方法  

   vue.js 则是采用数据劫持结合发布者-订阅者模式的方式，通过Object.defineProperty()来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。我们先来看Object.defineProperty()这个方法：

 