# 二：数据驱动

### 2-2：为什么使用this.message便可以直接得到data定义的message

> core/instance/states:  initState->initeData->getData->proxy
>
> data的初始化分析，即，为什么通过this.message可以访问到定义的数据：对定义的数据做了一层代理，访问this.message相当于访问this._data.message。
> 执行 new Vue的时候，会执行init方法，做一系列初始化操作。

### 2-3：vm.$mount做了什么事情

> platform/runtime-with-compiler
>
> 带compiler版本，备份Vue.prototype.$mount,再重写Vue.prototype.$mount，重写的mount目的是获得el对应的dom。从options中判断render，若没有则获取options中的模板，若没写模板，通过getOuterHTML拿到外部的DOM元素赋值给template，由template生成新的render函数。得到render函数后调用备份的mount方法，看该函数中的mountComponent函数，由render则渲染出VNode，updateComponent这片范围是关于性能的，updateComponent和渲染watcher有关系，noop空的函数。

### 2-4：render函数

> Src/core/instance/render.js
>
> vm._c是为被编译生成的render函数提供一个创建VNode的方法，vm.$createElement是为手写的render函数提供创建VNode的一个方法，render第二个参数,vm.$createElement,proxy对对象访问做一个劫持。render函数，VNode = render.call(_renderProxy, createElement)，先分析createElement, 然后看_renderProxy。createElement生成VNode。

### 2-6：讲解createElement函数

> update(vm._render(),hy…)-> _render->vnode = render(vm._renderProxy, vm.createElement)。createElement是对_createElement的封装，在_createElement将children打磨成一维数组后，再使用new Node生成VNode，完成了vm._render()。

### 2-7 讲解update

> 



# 三：组件化



> patch进入createElm进入createComponent进入createComponentInstanceForVnode进入vnode.componentOptions.ctor执行_init, 然后initInternalComponent合并参数，然后执行initlifecycle建立父子关系，退出createComponentInstanceForVnode，执行child.$mount进入mountComponent,然后_update，然后render.call，然后patch，createElm, createcomponent或一系列标签过程，createElement创建div，然后createChildren，其里面递归执行createElm,然后对于helloworld子组件，createcomponent，_update一系列从头开始的类似过程，createcomponent递归完后，从子组件开始插入父组件。
>
>  没有resolve，先渲染成注释节点，然后根据resolve和loading或error的状态相继渲染对应组件。



> 三步判断返回组件，生成res，使用res生成error和loading的ctor，然后根据有没有resolve，决定执不执行定时器，若执行定时器，则先执行loading的，loading置为true，forceRender（），经过一系列步骤，再进入resolveAsyncComponent，重复以上步骤，根据resolve和error状态返回组件或继续执行代码
> ![img](file:///C:\Users\秦世权\Documents\Tencent Files\2386590714\Image\C2C\]VQQ`Q%CI4%404P{HG`VDJT.png)



> 
>
> 异步组件工厂函数（）：先执行createComponent，然后ctor = resolveAsyncComponent，resolveAsyncComponent里面主要是三步判断->context->定义resolve和reject->(异步执行res = factory（）工厂函数-等待)->返回factory.resolved，第一次返回undefined，先生成注释节点->异步执行res = factory（）工厂函数，生成factory.resolved->执行forceRender->$forceUpdate->
> ![img](file:///C:\Users\秦世权\Documents\Tencent Files\2386590714\Image\C2C\)0]ABONY5`M(YR1}%}PY}GU.png)
> ->vm._render->再次执行createComponent重复以上步骤返回factory.resolved->后面的步骤和同步组件一样，最终成功patch出异步组件





# 四. 深入响应式(上)



> watcher作为订阅者，订阅数据的变化
>
> 案例过程:new watch()，this.get()，pushtarget，，update又会触发子组件的初始化进行子组件的mountcomponent，new watch，this.get()，pushtarget，value=this.getter.call()，getter就是updatecomponent，接着执行this._render，render的时候会跳到，，收集watcher，adddep到addsub，getter执行完后，poptarget和cleanupdeps

> 问题: cleanupdeps时，为什么不直接将deps清空，而是比对后再清空。







