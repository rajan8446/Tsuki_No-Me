
####    ******RULES******
1. Global context - this refers to window object.
2.  new keyword binding - the new keyword changes the meaning of this to be the object that is being created.
3. implicit binding - "this" refers to the object that is calling it. It is implied, without doing anything it's just how the language works.
4. explicit binding - using the "bind" keyword to change the meaning of "this".
5. arrow functions as methods - "this" is lexically scoped, refers to it's current
        surroundings and no further. However, if "this" is inside of a method's func on, it
        falls out of scope and belongs to the window object. To correct this, you can use a
        higher order func on to return an arrow func on that calls "this".

##### I/O -> O/P Questions

1.  
```
    this; // Window {...}
    window; // Window {...}
    this === window; // true
    function a() {
        console.log(this);
    }
    a(); // Window {...}
```

2. 
```
    const obj = {
        property: `I'm a property of obj.`,
        method: function () {
            // this refers to the object obj
            console.log(this.property);
        }
    };
    obj.method(); // I'm a property of obj.

    Note. this refers to whatever is on the left of the .(dot) when calling a method
```

3.
```
    var name = "window";
    function whichName() {
        console.log(this.name);
    }
    const obj1 = {
        name: "Obj 1",
        whichName
    };
    const obj2 ={
        name: "Obj 2",
        whichName
    };

    whichName(); // window
    obj1.whichName(); // Obj 1
    obj2.whichName(); // Obj 2
```

4.
```
    const a = function () {
        console.log("a", this);
        const b = function () {
            console.log("b", this);
            const c = {
                hi: function () {
                    console.log("c", this);
                }
            };
            c.hi(); // new obj c called function
        };
        b(); // ran by a window.a()b()
    };
    a(); // called by window
    // a Window {…}
    // b Window {…}
    // c {hi: ƒ}
```

5.
```
    function Person(name, age) {
        this.name = name;
        this.age = age;
        console.log(this);
    }
    const person1 = new Person("person1", 55);
    // this = Person { name: 'person1', age: 55 }
```

6. Implicit binding
```
    const person = {
        name: "person",
        age: 20,
        hi() {
            console.log("hi " + this);
        }
    };
    person.hi();
    // this = person { name: 'person', age: 20, hi(){...} }
```

7.  Explicit binding
```
    let name = "Brittney";
    const person3 = {
        name: "person3",
        age: 50,
        hi: function () {
            console.log("hi " + this.name);
        }.bind(window)
    };
    person3.hi();
    // hi Brittney
```    

8. Arrow functions inside objects
```
    const person4 = {
        name: "person4",
        age: 40,
        hi: function () {
            var inner = () => {
                console.log(this);
            };
            return inner();
        }
    };
    person4.hi();
    // this = person4 { name: 'person4', age: 40, hi() {...} }
    // if either function is changed around, it doesn't work
```

9. 
```    
    const obj = {
        name: "Billy",
        sing() {
            console.log("a", this);
            var anotherFunc = function () {
                console.log("b", this);
            };
            anotherFunc();
        }
    };
    obj.sing();
    // a {name: "Billy", sing: ƒ}
    // b Window {…}
```

10. 
```
const obj = {
    name: 'Amane',
    surname: 'Ubuyashiki',
    age: 25,
    printName: function() {
        console.log(`My Name: ${this.name} and age: ${this.age}`)
    }
}

obj.printName()
```

#####  Todo:  Now print the name after 1 second

    printName: function() {
        setTimeout(function () {
            console.log(`My Name: ${this.name} and age: ${this.age}`)
        })
    }
But this doesn't work. We get undefined.👆

    printName: function() {
        const that = this
        setTimeout(function () {
            console.log(`My Name: ${that.name} and age: ${that.age}`)
        })
    }

The above code works but it can still be simplified. 👆

    printName: function() {

        setTimeout(() => {
            console.log(`My Name: ${this.name} and age: ${this.age}`)
        })
    }

11.
```
    const obj = {
        name: "Billy",
        sing() {
            console.log("a", this);
            var anotherFunc = () => {
                console.log("b", this);
            };
            anotherFunc();
        }
    };
    obj.sing();
    // a {name: "Billy", sing: ƒ}
    // b {name: "Billy", sing: ƒ}
```

12.
```
    var b = {
        name: "jay",
        say() {
            console.log(this);
        }
    };
    var c = {
        name: "jay",
        say() {
            return function () {
                console.log(this);
            };
        }
    };
    var d = {
        name: "jay",
        say() {
            return () => console.log(this);
        }
    };
    b.say(); // b {name: 'jay', say()...}
    // b called the function
    c.say(); // function() {console.log(this)}
    // returned a function that gets called later
    c.say()(); // Window {...}
    // c.say() gets the function and the Window runs it
    d.say(); // () => console.log(this)
    // returned the arrow function
    d.say()(); // d {name: 'jay', say()...}
    // arrow function does not rebind this and inherits this from parent
```
https://dev.to/liaowow/take-this-quiz-understand-how-this-works-in-javascript-44dj



function aa() {
  let a = 10;
  console.log(this.a)
}
aa()
Output=> undefined // this will refer the global object, bcoz when funcion is not a property of any object then it will refers to the global object(window). and in the global there is no "a" defined, so it will give you undefined.

const bb = () => {
  let b = 20;
  console.log(this.b)
}
bb()
Output=> undefined //


const obj = {
  name: "rajan",
  age: 27,
  intro: function() {
    console.log(`My name is ${this.name}`)
  }
}
obj.intro() // output=>My name is rajan

const obj1 = {
  name: "rajan",
  age: 27,
  intro: () => {
    console.log(`My name is ${this.name}`)
  }
}
obj1.intro() // output=>undefined
Arrow functions do not have their own this context. Instead, they inherit this from the surrounding (lexical) context in which they were defined.
In your case, the intro method is defined as an arrow function inside the obj3 object. Since it's an arrow function, this inside the intro method will not refer to the obj3 object but to the this value of the context in which obj3 was created.



let name1 = "kumar";

const obj2 = {
  name: "rajan",
  age: 27,
  intro: function() {
    console.log(`My name is ${this.name1}`)
  }
}
obj2.intro();  output=> undefined
//this will give undefined bcoz name1 is not defined inside the obj2.

const obj3 = {
  name: "rajan",
  age: 27,
  intro: () => {
    console.log(`My name is ${this.name1}`)
  }
}
obj3.intro() // output=>My name is rajan.
//in arrow function this refers to surrounding (lexical) context in which they were defined. so this refers to the surrounding context that mean obj3 is defined in global object , and name1 is defined also in global. 

const obj4 = {
  name: "rajan",
  age: 27,
  intro: () => {
    console.log(`My name is ${this.name1}`)
  }
}
obj4.intro()
