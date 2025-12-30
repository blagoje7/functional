In imperative programming you write procedures (step 1, step 2, step 3), in functional programming you build pipelines.
To build pipelines, you need specific you need tools that dont really exist in imperative programming. 

# Currying and Composition.
## Tool 1:
## Currying ("Configuration tool")

### `The problem` : 
Most functions in JS take in multiple arguments at once. This makes them hard to combine.
```javascript
        //Imperative style
        const add = (a, b) => a + b;
        // You MUST have both arguments ready at the same time to call the function.
```
### `The solution` : 
Currying transforms a function so it takes arguments One at a time. It lets you confgure the function now and call it later.

```javascript

        //Curried version:
        //Type: Number -> Number -> Number
        const add = x => y => x + y;

        const addFive = add(5);

        //... 100+ lines of code

        console.log(addFive(10));
```
### `Why is this a weapon` :
It lets us creating specialized function from generic ones without rewriting the logic.

```javascript
        //Generic: 
        const checkPermission(role, user);
```
```javascript
        //Specialized:
        const isAdmin = checkPermission('admin') // now you can pass isAdmin to a fliter function easily.
```

## Tool 2:
## Composition ("The pipeline tool")
- The Math: $(f \circ g)(x) = f(g(x))$

### `The problem` : 
In imperative code we nest calls like onions, its very hard to read inside out.

```javascript
    //Not easy to read
    const result = toUpperCase(trim(getName(user)));
```
    
### `The solution (Composition)` : 
Lets us stick functions together like pipes. The output of one becomes the input of next.

```javascript

        //A simple composition function right to left Execution
        const compose = (f , g) => x => f(g(x));

        //Usage
        const getName = name => user.name;
        const upperCase = str => str.toUpperCase();

        //We build new function by sticking two others together
        const getUpperName = compose(upperCase, getName);

        console.log(getUpperName({name : "haskell" }));
```

### Rules of arrow function parentheses:
#### Parentheses:
- `0 Arguments` : Parentheses are **mandatory**. `const sayHi = () => console.log('Hi');`
- `1 Argument` : Parentheses are **optional**. `x => x * 2` is the same as `(x) => x * 2`
- `2+ Arguments` : Parentheses are **mandatory**. `const add = (a, b) => a + b;`

## Tool 3:
## filter : ("The bouncer")

The concept `filter` is like the night club bouncer. It takes list and **decision-making**  function (a predicate). It runs every item through the function. 

>- If function return `true` item stays on the new list
>- If function return `false` gets kicked out of the new list.

- `The rule` : output list is alwaysd shorter or same length as input and items themselves do not change.

#### *Imperative v Functional*
>` Task `: Keep only the even numbers

```javascript

//Imperative
const numbers = [1, 2, 3, 4, 5, 6, 7, 8];
const evens = []; // create empty container

for(let i = 1; i < numbers.length; i++){
    if (numbers[i] % 2 == 0){ //the logic
        evens.push(numbers[i]); //the mutation
    }
}
```

```javascript
//Functional
const numbers = [1, 2, 3, 4, 5, 6, 7, 8];

//Decision logic (The predicate)
//Type : (Number) -> Boolean
const isEven = num => num % 2 === 0;
//The usage
const evens = numbers.filter(isEven);
//The result: [2 ,4, 6]
 
```

#### How this works ?
- `You`: Hand over `isEven` to the `filter`.
- `Filter` : "Ill run this logic on every item. If it beeps `true` ill keep the item.

## Tool 4: 
## Reduce  ("The swiss army knife")
`The concept` : `reduce` is most powerfool tool of them all. It takes `list` and combines it into the single value.

- The "single value" number (sum), object or another array!
- **Fun fact** : You can actually build `map` and `filter` using `reduce`.

**Key difference** : `filter` and `map` look at items one by one in isolation. `reduce` carries a memory (Accumulator) from one step to another.

### *Imperative vs Functional*

#### Imperative

```javascript

const numbers = [1, 2, 3, 4];
let sum = 0; //The accumulator starts at 0

for(let i = 0; i<numbers.length; i++){
    sum = sum + numbers[i];
}

```

#### Functional
`reduce` needs two things:
- **The combiner function** : How to merge current item into the memory. `(memory, item) => newMemory`
- **The initial value** : There does the memory start ? (e.g. 0)
```javascript
const numbers = [1, 2, 3, 4];
//The combiner logic
//(Accumulater, Current) -> New Accumulator
const add = (acc, current) => acc + current;

//The usage
//.reduce(logic, initialValue)
const tool = numbers.reduce(add, 0);
//Result: 10
```

#### Walkthrough the execution

1. Start : `acc = 0`;
2. Step 1 : `add(0, 1)` Returns : `1` new `acc = 1`;
3. Step 2 : `add(1, 2)` Returns : `3` new `acc = 3`;
4. Step 3 : `add(3, 3)` Returns : `6` new `acc = 6`;
5. Step 2 : `add(6, 4)` Returns : `10` new `acc = 10`;