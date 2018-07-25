### 1.关于js的深拷贝以及浅拷贝
  浅拷贝只复制指向某个对象的指针，而不复制对象本身，新旧对象还是共享同一块内存。但深拷贝会另外创造一个一模一样的对象，新对象跟原对象不共享内存，修改新对   象不会改到原对象。
### 2.浅拷贝的实现方式
  （1）也就是简单地复制而已
  ```  
   function simpleClone(initalObj) {    
      var obj = {};    
      for ( var i in initalObj) {
        obj[i] = initalObj[i];
      }    
      return obj;
    }
```
 （2）Object.assign()进行的是浅拷贝，拷贝的是对象的属性的引用，而不是对象本身。需要注意的是：Object.assign()可以处理第一层的深度拷贝
### 3.深拷贝的实现方式
  (1)json方式，但是这种方法也有不少坏处，譬如它会抛弃对象的constructor。也就是深拷贝之后，不管这个对象原来的构造函数是什么，在深拷贝之后都会变Object。
这种方法能正确处理的对象只有 Number, String, Boolean, Array, 扁平对象，即那些能够被 json 直接表示的数据结构。RegExp对象是无法通过这种方式深拷贝。
也就是说，只有可以转成JSON格式的对象才可以这样用，像function没办法转成JSON
``` 
   var obj2 = JSON.parse(JSON.stringify(obj1));
 ```
  （2）递归的简单实现版本
```
  function deepClone(obj){
     let newObj = obj.constructor === Array ? [] : {};  //首先判断传入的对象是数组还是对象
     for(key in obj){
        //如果是对象以及数组则递归，否则直接赋值
        newObj[key] = typeof obj[key] === 'object' ? deepClone(obj[key]) :  obj[key]; 
     }
     return newObj;
  }
  ```
### 4.call和apply的异同
```
function func (a,b,c) {}
func.call(obj, 1,2,3)
//求数组的最大值
var max = Math.max.apply(null, ary); 
//类数组转为数组
var ary = [].slice.call(arguments);
```
    这两个方法其实功能是一样的，都能够改变方法的执行上下文（执行环境）this，将一个对象的方法交给另一个对象来执行，并且是立即执行。
    默认情况下，this指是调用该方法的对象
    只是参数不一致，call传递参数的时候，是一个个的传递值的，而apply参数为数组或者类数组。
 
### 5.类数组对象
    1.通过角标调用，如 array[0]
    2.长度属性length
    3.通过 for 循环和forEach方法进行遍历
```
 var arrayLike = {
    0: 'item1',
    1: 'item2',
    2: 'item3',
    length: 3
  }
  [].forEach.call(arrayLike, (item) => console.log(item)
```
### 6.AMD与CMD以及CommonJs的区别
    AMD的代表是RequireJS,遵循的是依赖前置,异步加载模块，必须要提前加载所有的依赖
    CMD的代表是SeaJS,遵循的规范是依赖就近,延迟执行，通过按需加载的方式
    CommonJs主要用与服务器端，nodejs等，是同步的，只有加载完成后才能执行后面的操作.
    commonJS是运行时加载,ES6模块化是编译时加载.
### 7.关于this
    在《javaScript语言精粹》这本书中，把 this 出现的场景分为四类，简单的说就是：
    有对象就指向调用对象
    没调用对象就指向全局对象
    用new构造就指向新对象
    通过 apply 或 call 或 bind 来改变 this 的所指。
### 8.为什么canvas明明高度设置的是父元素的同等高度，就是有滚动条呢？
    原来canvas设置了行内块级元素,inline-block的元素之间会存在“4px”的空白间距,这里只要设置父元素的font-size为0就可以了。
    然后重置子元素的fontsize.
### 9.关于继承
```
    //parent构造函数
    function Parent(name) {
      this.name = name || 'Parent';
    }

    //给原型增加方法
    Parent.prototype.say = function () {
      return this.name;
    };

    //空的child构造函数
    function Child(name) {}

    //继承
    inherit(Child, Parent);

```
    1.类式继承
    (1)默认模式，这种模式的一个缺点是既继承了（父对象）“自己的属性”，也继承了原型中的属性。大部分情况下你可能并不需要“自己的属性”，因为它们更可能是为实例对象添加的，并不用于复用。
```
    function inherit(C, P) {
      C.prototype = new P();
    }
```  
    (2)借用构造函数.这种模式的一个明显的弊端就是无法继承原型。原型往往是添加可复用的方法和属性的地方，这样就不用在每个实例中再创建一遍。
      这种模式的一个好处是获得了父对象自己成员的拷贝，不存在子对象意外改写父对象属性的风险。
```
   function Child(a, c, b, d) {
      Parent.apply(this, arguments);
    }
``` 
    (3)共享原型.不太实用，子对象修改原型都有影响
  ```
   function inherit(C, P) {
    C.prototype = P.prototype;
  }
```   
    (4)临时构造函数.这种模式通常情况下都是一种很棒的选择，因为原型本来就是存放复用成员的地方。
       在这种模式中，父构造函数添加到this中的任何成员都不会被继承。要记得重置constructor构造函数应该
       为方法本身。
  ```
   function inherit(C, P) {
      var F = function () {};
      F.prototype = P.prototype;
      C.prototype = new F();
      C.prototype.constructor = C;
    }
```  
    2.原型继承,其实就是 Object.create()方法;
  ```
   function object(o) {
      function F() {}
      F.prototype = o;
      return new F();
    }
```  
    3.复制继承,深浅copy
