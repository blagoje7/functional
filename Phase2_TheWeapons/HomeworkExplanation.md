```javascript
const transform = (y) => (y*2)+1;
```
Here we are passing one argument in arrow function that doubles that argument  and then adds one to product of multiplication
-----------------------------
```javascript
const processNumbers = (nums) => nums.map(transform);
```
Here we again are passing one argument that then gets applied map with our already defined transform function transform, nevermind that we do not have `transform` (argument) it will do transform with whatever is currently in iteration of map function because thats just how map() works.

This is what .map() does internally
```javascript
Array.prototype.map = function(callbackFn) {} 
```
'callbackFn' is the name 'map' gives to your 'transform' function you pass as argument

```javascript

{
  const result = [];
  
  for (let i = 0; i < this.length; i++) {
    const currentValue = this[i]; // It grabs the value (e.g., 1)
    
    // HERE IS THE MAGIC!
    // 'map' manually adds the parentheses and passes the value for you.
    const newValue = callbackFn(currentValue); 
    
    result.push(newValue);
  }
  
  return result;
}

```
1. You pass transform to map.

2. map renames it callbackFn internally.

3. map runs the loop.

4. Inside the loop, map writes the parentheses ().

5. map puts the current array item (this[i]) inside those parentheses.

`map` function iterates through array and for every element it applies function passed in argument of map(here) and then returns NEW array back without mutating old one

This is "sort of composition", we pass one argument
 
 In this case array it would fail if it wasnt array 
  > if we passed number to map, in this case processNumbars(123)
  > JS tries to find .map of number 123  > it cant find it
  > Error: Uncaught TypeError

In other exampe if it was array of arrays
Since JS is wekly typed language it wont warn us fo error right away 
rather it would try [1,2] * 2 - which doesnt exist in JS so result would be `NaN`
resulting array would be `[Nan, Nan]` ( if it was some operation that could be done on arrays we would get result just not right one )
-----------------------------
```javascript
console.log(processNumbers([1, 2, 3, 4]));
// this takes that passed array iterates through it and applies our transform fn 
// to every element
```
