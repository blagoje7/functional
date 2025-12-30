# Algebraic Structures

> In phase 2 we were talking about `pipelines` in phase 3 we are talking about `protecting those pipelines`

In imperative code safety looks like this `(Defensive code)`

```javascript

if(user !== null && user.balance !== undefined ) { ... }

```

On the other hand in *`functional programming`*  we do not use `if` checks. We wrap dangerous values in a **Container** that handles checks for us.

#### The problem: "The billion dollar mistake"

In prevous example if user is null pipeline would just crash
- `Imperative fix` : add `try/catch` or `if (x)` everywhere
- `FP fix` : Never pass raw value. Pass a container `Monad` that holds the value.

### Step 1: Building 'maybe' container from scratch

We are going to build 'Maybe' monad from scratch, it has two states:
- `Just(x)` : It has value `x`.
- `Nothing()` : It has no value, empty (null/undefined).

```javascript

// 1. The "Just" container (holds a value)
const Just = val => ({
    //map applies function and re-wraps the result in Just
    map: f => Just(f(val)),

    //chain allows us to switch from Just -> Nothing if needed
    chain: f => f(val),

    //helper to let us see whats inside
    inspect: () => `Just(${val})`

});

//2. The "Nothing" container (Holds nothing)
const  Nothing = () => ({
    //map does nothing, ignores function f
    map : f => Nothing(),

    //chain does nothing
    chain : f => Nothing(),

    inspect : () => 'Nothing'

})

//3. The factory (decides which one to use)
const Maybe = (x) => ( x === null || x === undefined) ? Nothing() : Just(x);

```

#### The magic:

> Notice `Nothing.map`. It ignores the function `f`. **This is how we avoid crashes!** If you try to map over `null` value wrapped in `Nothing` it just skips it.

### Step 2: The crash proof pipeline

Lets break previous code on purpose then fix it with `Maybe`.

#### The broken data

```javascript

const user = {
    id : 1,
    name : 'Alice',
    address : null
}

```

##### Task: get street name in uppercase

Attempt 1: (Standard JS)

```javascript

const getStreetName = user => user.address.street; //Error: cannot read 'street' of null
const toUpper = str => str.toUpperCase();

//Usage
toUpper(getStreetName(user));  //everything breaks here

```

Attempt 2: (With Maybe - Safe) : We rewrite our helpers to be 'Monad-friendly' or just swap data 

```javascript

//Safe helpers with Maybe
//prop: returns Maybe (Just value or Nothing)
const safeProp = key => obj => Maybe(obj[key]);

//The logic
const getStreet = safeProp('address');         //returns: Mayne(address)
const getStreetName = safeProp('street');   //returns: Maybe(street)

//The execution
const result = Maybe(user)                 //Wrap the user: Just(user)
    .chain(getStreet)                             // Try to get address: Nothing() because its null
    .chain(getStreetName)                   // Skipped: Still nothing!
    .map(str => str.toUpperCase());     // Skipped: Still nothing!

console.log(result.inspect()); // "Nothing"

```

#### Why is it amazing?

1. **No** `if` **statements** : Logic flowed from start to finish.
2. **No crash** : The `Nothing` container acted lik a shield. It sucessfully absorbed the error and safely propagated `"Nothing"` to the end.

