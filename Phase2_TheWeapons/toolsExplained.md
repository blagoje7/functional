# Explanation

## What went wrong?

> Inside commented section.

### 1. Predicate Mismatch
- `I wrote` : `findActive(users, 'active')
- `The issue` : `lists.filter` expects a **function** that returns *true* or *false*. It got string `active`.
- `The fix` : I created `const isActive = user => user.status === 'active';`. Now filter has function to call.

### 2. `map` Mismatch
- `I wrote` : `map('balance')`
- `The issue` : `map` expects a function that transforms data. it got string `balance`.
- `The fix` : We created `const getBalance = user => user.balance;`

### 3. The Empty Reduce
- `I wrote` : `reduce()`
- `The issue` : `reduce` needs instructions on how to combine things, it cannot guess you want to sum them.
- `The fix` : Passed add ( ` the logic` ) and 0 (`the starting point`)

#### Pro move (add into tools.html)

```javascript

//generic helpers (The "Factory" functions)
const prop = key => obj => obj[key];                        // Turns 'balance' into a function
const eq = key => val => obj => obj[key] === val;   // Turns 'active' into function

const total = users.filter(eq('status')('active')).map(prop('balance')).reduce((a, b) => a + b, 0);

```
