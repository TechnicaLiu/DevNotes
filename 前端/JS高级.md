# 原型/原型链

## 函数的原型 

两者比较着看：

prototype : 原型  =》   函数的一个属性

`__proto__` 原型链（连接点）  =》 对象Object的一个属性 

**对象的 `__proto__`  保存着该对象的构造函数的prototype**   



 每个函数都会有一个prototype属性 。即显示原型属性，默认指向一个空的Object对象 

每个实例对象都有一个`__proto__` ,可称为隐式原型

**对象的隐式原型的值为其对应构造函数的显示原型的值** 

<img src="https://gitee.com/youngstory/images/raw/master/img/202110252013455.png" alt="image-20211025201341306"  />

```js
 function Animals() {
      
    }
 var cat = new Animals(); // this指向 实例对象
 console.log(cat.__proto__ === Animals.prototype); // true 
```

```js
 function Animals() {
      this.a = 1;

    }
    var cat = new Animals(); // this指向 实例对象
    Animals.prototype.b = 2;
    Object.prototype.c = 3;
    console.log(cat.a); // 1 
    console.log(cat.b); // 2  自身没有b属性就会沿着原型链往上找
```

Function和 Object 

```js
 console.log(Function.__proto__); // ƒ () { [native code] }
    console.log(Function.prototype); // ƒ () { [native code] } 
    //  Function.__proto__ ===  Function.prototype
    console.log(Object.__proto__ === Function.prototype); // true 
```

## **判断属性是否存在 ？** 

```js
 function Test() {
      this.a = 1;
      this.b = 2
    }
    Test.prototype.c = 3;
    var demo = new Test();
    console.log(demo.hasOwnProperty('c')); // 不包含原型链上的 只是自身属性  
    console.log('c' in demo);  // 包含原型链 
```

constructor  指向实例的构造函数 ，是允许更改的

## **原型链继承**

优点：父类新增方法属性，子类都能访问到，简单易于实现

缺点：**子类的实例共享了父类构造函数的引用属性**  不能传参

```js
// 定义一个动物类 
function Animal(name) {
  this.name = name || 'Animal';
  // 实例方法
  this.sleep = function () {
    console.log(this.name + '正在睡觉');
  }

}
// 原型方法
Animal.prototype.eat = function (food) {
  console.log(this.name + '正在吃' + food);
}

// 原型链继承  
// 将父类的实例作为子类的原型 
// 缺点： 子类的实例共享了父类构造函数的引用值(数组 对象等)  不能传参
function Cat() {

}
Cat.prototype = new Animal();
Cat.prototype.name = 'cat';

var cat = new Cat();
cat.eat('fish');//cat正在吃fish
cat.sleep();//cat正在睡觉
```

## 构造函数继承

优点：子类构造函数 借用 父类的构造函数   解决了引用值共享的问题

缺点： 没有办法拿到原型上的方法；

```js
 // 由于原型链继承  Cat.prototype = new Animals()  存在引用共享的问题 所以 采用构造函数 
    // 借用构造函数继承  
    function Animals() {
      // this.a = 1;
      this.a = [1, 2, 3, 4, 5]
    }
    Animals.prototype.say = function () {
      console.log('会叫');
    }

    function Cat() {
      Animals.call(this);
    }

    var cat1 = new Cat();
    var cat2 = new Cat();
    cat1.a.push(6);
    console.log(cat1.a); // 1 2 3 4 5 6
    console.log(cat2.a); // 1 2 3 4 5
```

## **组合继承**（伪经典继承）

优点：可传参   

缺点：调用了两次父类的构造函数，造成了不必要的消耗

```js
// 组合继承
// 构造函数继承+ 原型链继承 
// 缺点： 调用了两次父类的构造函数，造成了不必要的消耗，父类方法可复用 
// 优点可传参，不共享父类引用属性 
    function Animals() {
      // this.a = 1;
      this.a = [1, 2, 3, 4, 5]
    }
    Animals.prototype.say = function () {
      console.log('会叫');
    }

    function Cat() {
      Animals.call(this);
    }
    Cat.prototype = new Animals();
    var cat1 = new Cat();
    var cat2 = new Cat();
    console.log(cat1);
    cat1.a.push(6);
    cat1.say() // 会叫
    console.log(cat2.a); // 1 2 3 4 5
```

## **寄生组合继承**（经典继承）

优点：可传参，不会创建多余的实例占用内存 ，可通过Object.assign实现多继承

缺点：

```js
// 寄生组合继承 
function Cat(name,age){
  Father.call(this,name);
  this.age=age
}
Cat.prototype=Object.create(Animal.prototype)
Cat.prototype.constructor=Cat
var cat = new Cat('MING', 20)
console.log(cat);
```

## **ES6的extend**

```js
class Animal {
	constructor (name) {
	this.name = name
	}
	showName () {
		alert(this.name)
	}
}
class Cat extends Animal {
	constructor (name) {
		super(name)
	}
	sayMy () {
		super.showName()
	}
}

```

