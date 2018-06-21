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
        newObj[key] = typeof obj[key] === 'object' ? deepClone(obj[key]) :  obj[key]; //如果是对象以及数组则递归，否则直接赋值
     }
     return newObj;
  }
  ```
### 4.call和apply的异同
```
function func (a,b,c) {}
func.call(obj, 1,2,3)
func.call(obj, [1,2,3])
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
 
