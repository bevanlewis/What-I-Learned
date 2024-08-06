# JavaScript Arrays

Arrays are used to store multiple values in a single variable.

```javascript
let fruits = ["apple", "banana", "orange"];
```

## Accessing Array Elements

You can access array elements using their index.

```javascript
let fruits = ["apple", "banana", "orange"];
console.log(fruits[0]); // "apple"
console.log(fruits[1]); // "banana"
console.log(fruits[2]); // "orange"
```

## Array Length

The `length` property returns the number of elements in an array.

```javascript
let fruits = ["apple", "banana", "orange"];
console.log(fruits.length); // 3
```

## Array Methods

Array methods are used to manipulate arrays.

Methods described in this section:

- `push`
- `pop`
- `shift`
- `unshift`
- `concat`
- `slice`
- `splice`
- `join`
- `sort`
- `reverse`
- `indexOf`
- `forEach`
- `map`
- `filter`
- `reduce`
- `find`
- `findIndex`
- `every`
- `some`
- `includes`

### Array Push and Pop

The `push` method adds one or more elements to the end of an array and returns the new length of the array. The `pop` method removes the last element from an array and returns that element.

```javascript
let fruits = ["apple", "banana", "orange"];
fruits.push("grape");
console.log(fruits); // ["apple", "banana", "orange", "grape"]
let lastFruit = fruits.pop();
console.log(lastFruit); // "grape"
```

### Array Shift and Unshift

The `shift` method removes the first element from an array and returns that element. The `unshift` method adds one or more elements to the beginning of an array and returns the new length of the array.

```javascript
let fruits = ["apple", "banana", "orange"];
let firstFruit = fruits.shift();
console.log(firstFruit); // "apple"
console.log(fruits); // ["banana", "orange"]
fruits.unshift("pear");
console.log(fruits); // ["pear", "banana", "orange"]
```

### Array Concat

The `concat` method is used to merge two or more arrays. It does not change the existing arrays, but instead returns a new array.

```javascript
let fruits = ["apple", "banana", "orange"];
let moreFruits = ["grape", "kiwi"];
let allFruits = fruits.concat(moreFruits);
console.log(allFruits); // ["apple", "banana", "orange", "grape", "kiwi"]
```

### Array Slice and Splice

The `slice` method returns a shallow copy of a portion of an array into a new array object. The `splice` method changes the contents of an array by removing or replacing existing elements and/or adding new elements in place.

```javascript
let fruits = ["apple", "banana", "orange"];
let citrusFruits = fruits.slice(1, 3);
console.log(citrusFruits); // ["banana", "orange"]
let removedFruits = fruits.splice(1, 2, "pear", "grape");
console.log(removedFruits); // ["banana", "orange"]
console.log(fruits); // ["apple", "pear", "grape"]
```

### Array Join

The `join` method creates and returns a new string by concatenating all of the elements in an array, separated by a specified separator.

```javascript
let fruits = ["apple", "banana", "orange"];
let fruitsString = fruits.join(", ");
console.log(fruitsString); // "apple, banana, orange"
```

### Array Sort

The `sort` method sorts the elements of an array in place and returns the sorted array.

```javascript
let fruits = ["apple", "banana", "orange"];
fruits.sort();
console.log(fruits); //
// ["apple", "banana", "orange"]
```

### Array Reverse

The `reverse` method reverses the order of the elements in an array in place.

```javascript
let fruits = ["apple", "banana", "orange"];
fruits.reverse();
console.log;
```

### Array IndexOf

The `indexOf` method returns the index of the first occurrence of a specified value in an array.

```javascript
let fruits = ["apple", "banana", "orange"];
console.log(fruits.indexOf("banana")); // 1
```

### Array ForEach

The `forEach` method executes a provided function once for each array element.

```javascript
let fruits = ["apple", "banana", "orange"];
fruits.forEach((fruit) => console.log(fruit));
// "apple"
// "banana"
// "orange"
```

### Array Map

The `map` method creates a new array with the results of calling a provided function on every element in the calling array.

```javascript
let numbers = [1, 2, 3, 4, 5];
let doubled = numbers.map((num) => num * 2);
console.log(doubled); // [2, 4, 6, 8, 10]
```

### Array Filter

The `filter` method creates a new array with all elements that pass the test implemented by the provided function.

```javascript
let ages = [32, 33, 16, 40];
let adults = ages.filter((age) => age >= 18);
console.log(adults); // [32, 33, 40]
```

### Array Reduce

The `reduce` method applies a function against an accumulator and each element in the array (from left to right) to reduce it to a single value.

```javascript
let numbers = [1, 2, 3, 4, 5];
let sum = numbers.reduce((acc, curr) => acc + curr, 0);
console.log(sum); // 15
```

### Array Find

The `find` method returns the value of the first element in the array that satisfies the provided testing function.

```javascript
let numbers = [5, 12, 8, 130, 44];
let found = numbers.find((num) => num > 10);
console.log(found); // 12
```

### Array FindIndex

The `findIndex` method returns the index of the first element in the array that satisfies the provided testing function.

```javascript
let fruits = ["apple", "banana", "orange", "grape"];
let index = fruits.findIndex((fruit) => fruit === "orange");
console.log(index); // 2
```

### Array Includes

The `includes` method determines whether an array includes a certain value among its entries, returning true or false as appropriate.

```javascript
et fruits = ['apple', 'banana', 'orange'];
console.log(fruits.includes('banana')); // true
console.log(fruits.includes('grape')); // false
```

### Array Every

The `every` method tests whether all elements in the array pass the test implemented by the provided function.

```javascript
let numbers = [2, 4, 6, 8, 10];
let allEven = numbers.every((num) => num % 2 === 0);
console.log(allEven); // true
```

### Array Some

The `some` method tests whether at least one element in the array passes the test implemented by the provided function.

```javascript
let numbers = [1, 3, 5, 7, 9];
let hasEven = numbers.some((num) => num % 2 === 0);
console.log(hasEven); // true
```

## Array Iteration

You can iterate over an array using a `for` loop.
Check [Loops](Loops.md) for more info on how loops work.

```javascript
let fruits = ["apple", "banana", "orange"];
for (let i = 0; i < fruits.length; i++) {
  console.log(fruits[i]);
}
```

## Array Spread Operator

The spread operator (`...`) allows you to expand an array into its individual elements.

```javascript
let fruits = ["apple", "banana", "orange"];
let moreFruits = ["grape", "kiwi"];
let allFruits = [...fruits, ...moreFruits];
console.log(allFruits); // ["apple", "banana", "orange", "grape", "kiwi"]
```

## Array Destructuring

Array destructuring allows you to extract values from arrays and assign them to variables.

```javascript
let fruits = ["apple", "banana", "orange"];
let [first, second, third] = fruits;
console.log(first); // "apple"
console.log(second); // "banana"
console.log(third); // "orange
```

### Array Destructuring with Spread Operator

You can use array destructuring with the spread operator to extract values from arrays and assign them to variables.

```javascript
let fruits = ["apple", "banana", "orange"];
let [first, ...rest] = fruits;
console.log(first); // "apple"
console.log(rest); // ["banana", "orange"]
```

## Comparing Arrays

You can not compare arrays using the `===` operator. When using the `===` operator, it will return `false` even if the arrays have the same elements in the same order, this is because the arrays are stored in different memory locations and javascript compares the memory locations of arrays.

```javascript
let fruits1 = ["apple", "banana", "orange"];
let fruits2 = ["apple", "banana", "orange"];
console.log(fruits1 === fruits2); // false
```

To compare arrays, you can use the `every` method to check if all elements in the arrays are equal.

```javascript
let fruits1 = ["apple", "banana", "orange"];
let fruits2 = ["apple", "banana", "orange"];
console.log(fruits1.every((element, index) => element === fruits2[index])); // true
```
