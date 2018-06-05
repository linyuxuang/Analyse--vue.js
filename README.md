# Analyse--vue.js
剖析 Vue.js 内部运行机制

 响应式系统的基本原理
  
    Object.defineProperty
     首先我们来介绍一下 Object.defineProperty，Vue.js就是基于它实现「响应式系统」的。    

     首先是使用方法：
     
         /*
          obj: 目标对象
          prop: 需要操作的目标对象的属性名
          descriptor: 描述符

          return value 传入对象
        */
        Object.defineProperty(obj, prop, descriptor)...

        descriptor的一些属性，简单介绍几个属性，具体可以参考 MDN 文档。

          enumerable，属性是否可枚举，默认 false。
          configurable，属性是否可以被修改或者删除，默认 false。
          get，获取属性的方法。
          set，设置属性的方法。...

     
                  function cb (val) {
                      /* 渲染视图 */
                      console.log("视图更新啦～");
                  }
                  function defineReactive (obj, key, val) {
                      Object.defineProperty(obj, key, {
                          enumerable: false,       /* 属性可枚举 */
                          configurable: true,     /* 属性可被修改或删除 */
                          get: function() {
                              return val;         /* 实际上会依赖收集，下面会讲 */
                          },
                          set: function(newVal) {
                              if (newVal === val) return;
                              cb(newVal);
                          }
                      });
                  }
                  function observer (value) {
                      if (!value || (typeof value !== 'object')) {
                          return;
                      }
                      Object.keys(value).forEach(function(key) {
                          defineReactive(value, key, value[key]);
                      });
                  }
                  class Vue {
                      /* Vue构造类 */
                      constructor(options) {
                          this._data = options.data;
                          observer(this._data);
                      }
                  }
                  let o = new Vue({
                      data: {
                          test: "I am test."
                      }
                  });
                  o._data.test = "hello,world.";  /* 视图更新啦～ */
     
     
     
     
     


