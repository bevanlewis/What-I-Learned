# JavaScript Classes

Classes are used to create objects with similar properties and methods.

## Creating a class

To create a class, use the `class` keyword followed by the name of the class. Initializing the class requires using the `new` keyword.

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  sayHello() {
    console.log(
      `Hello, my name is ${this.name} and I am ${this.age} years old.`
    );
  }
}

const person = new Person("John", 30);
person.sayHello(); // "Hello, my name is John and I am 30 years old."
```

## Inheritance

Classes can inherit from other classes using the `extends` keyword.

```javascript
class Animal {
  constructor(name) {
    this.name = name;
    this.age = 0;
    this.isHungry = false;
  }
}
class Dog extends Animal {
  constructor(name) {
    super(name);
    this.breed = "Golden Retriever";
  }
}
const dog = new Dog("Fido");
console.log(dog.name); // "Fido"
console.log(dog.breed); // "Golden Retriever"
```

### Checking inheritance

To check if an object is an instance of a class, use the `instanceof` operator.

```javascript
class Pet {
  constructor(name) {
    this.name = name;
  }

  introduce() {
    console.log(`This is my pet, ${this.name}.`);
  }
}

class Dog extends Pet {}
const dog = new Dog("Fido");
console.log(dog instanceof Dog); // true
console.log(dog instanceof Pet); // true
```

```javascript
const dog = new Dog("Otis");

Dog.prototype.isPrototypeOf(dog); // => true
Pet.prototype.isPrototypeOf(dog); // => true
Pet.prototype.isPrototypeOf(Dog.prototype); // => true

Pet.prototype.hasOwnProperty("introduce"); // => true
Dog.prototype.hasOwnProperty("introduce"); // => false
dog.hasOwnProperty("introduce"); // => false
```
