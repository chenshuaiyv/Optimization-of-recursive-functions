Optimization of recursive functions
# 递归函数的优化

递归函数是一个函数自我调用而构成的，如下是一个典型的递归阶乘函数：
```
function factorial(num){
  if(num<=1){
    return 1;
  }else{
    return num*factorial(num-1);
  }
}
```
这个函数当然没有什么问题，但遇到下面的情况时，却出现了问题：
```
var newFactorial = factorial;
factorial=null;
alert(factorial(5));
```

此时会报错：
>Exception: TypeError: factorial is not a function

为什么会出现这种问题呢？原因就出在`return num*factorial(num-1)`这一句上，这种写法使得函数太过紧密，一旦将函数保存到另一个变量中，并将原变量设置为null,factorial便不再是函数，因此会报错。

**解决方法：arguments.callee**

arguments.callee是一个指向正在执行的函数的指针，修改后代码如下：
```
function factorial(num){
  if(num<=1){
    return 1;
  }else{
    return num*arguments.callee(num-1);
  }
}
```

这样就实现了更松散的耦合，解决了问题。

当然，还有另外一种方式：
```
var factorial=(function f(num){
  if(num<=1){
    return 1;
  }else{
    return num*f(num-1);
  }
})
```

上述方法创建了一个函数名为 f 的表达式，并将其赋值给factorial，这样一来即便将函数赋值给其他变量，函数名 f 依然有效。