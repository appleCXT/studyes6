# studyes6
## promise
###参考资料
* https://www.cnblogs.com/whybxy/p/7645578.html
*
```
var p = new Promise(function(resolve, reject){
    setTimeout(function(){
        console.log('在newpromise后，等待5秒后执行打印');
        resolve('传给then的resolve函数的参数');
    }, 5000);
});
//运行代码，在2秒后执行打印。虽然只是new了一个对象p，并没有调用它，回调函数就已经执行了。
一般用法是：
function runAsync(){
    var p = new Promise(function(resolve, reject){
        //做一些异步操作
        setTimeout(function(){
            console.log('执行完成');
            resolve('随便什么数据');
        }, 2000);
    });
    return p;            
}
runAsync()
//包装好的函数后执行这个函数我们得到了一个Promise对象。
runAsync().then(function(data){ 
//这时回调函数的参数data是promise对象p的resolve（）中的参数。回调函数在runAsync这个异步任务执行完成之后被执行<br/>
//这里所说的runAsync这个异步任务执行完成是指：从pending变为fulfilled和从pending变为rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）
    console.log(data);
    //后面可以用传过来的数据做些其他操作
    //......
});
* 实质上，Promise的精髓是“状态”,只有在状态变为 resolved（已定型）才会执行then中的回调方法。<br/>
* 在链式操作中，后一个then会取得前一个then返回的promise对象(里面的属性和方法也传递)，前一个then对象会传递自己的resolve参数给后一个then<br/>
runAsync1()
.then(function(data){
    console.log(data);
    return runAsync2();
})
.then(function(data){
    console.log(data);
    return runAsync3();
})
.then(function(data){
    console.log(data);
});
//异步任务1执行完成<br/>
//随便什么数据1<br/>
//异步任务2执行完成<br/>
//随便什么数据2<br/>
//异步任务3执行完成<br/>
//随便什么数据3<br/>

function runAsync1(){
    var p = new Promise(function(resolve, reject){
        //做一些异步操作
        setTimeout(function(){
            console.log('异步任务1执行完成');
            resolve('随便什么数据1');
        }, 1000);
    });
    return p;            
}
function runAsync2(){
    var p = new Promise(function(resolve, reject){
        //做一些异步操作
        setTimeout(function(){
            console.log('异步任务2执行完成');
            resolve('随便什么数据2');
        }, 2000);
    });
    return p;            
}
function runAsync3(){
    var p = new Promise(function(resolve, reject){
        //做一些异步操作
        setTimeout(function(){
            console.log('异步任务3执行完成');
            resolve('随便什么数据3');
        }, 2000);
    });
    return p;            
}
* promise的resolve()是在所有的同步代码执行完后最后执行的
//参考资料：https://blog.csdn.net/github_26672553/article/details/53762054
let promise = new Promise(function(resolve,rejeact){//Promise新建后就会立即执行
  console.log('Promise');
  resolve();
});

promise.then(function(){
  console.log('Resolved');
});

console.log('Hi');

// Promise
// Hi
// Resolved
//Promise新建后立即执行，所以首先输出的是”Promise”，之后then方法指定的回调函数将在当前脚本所有同步任务执行完才会执行，所以”Resolved”最后输出。
```
