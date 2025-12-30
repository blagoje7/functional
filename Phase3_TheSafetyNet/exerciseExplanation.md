## What went wrong?

#### 1. Typo in `Maybe`

Using x in 
>Nothing() : Just(x) 
- `The crash` : `ReferenceError : x is not declared`

#### 2. The "Function" vs "Result"

safeProp is curried : `key => obj => result`
I wrote const `getBio = user => safeProp('bio')`
- `The issue` : This function ignores `user` input and returns inner function waiting for an object. My chain will pass `Just( [Function] )` instead of `Just(bio)`
- `The fix` : **Call it!** 

```javascript 
const getBio = user => safeProp('bio')(user);
```
or simply (Poing-free style)
```javascript
const getBio = user => safeProp('bio');
```

#### 3. Premature execution (in map())
i wrote : `.map(getFirstLetter())` 
- `The crash` : I called function imidiately without the arguments. It will try to execute it right away. `undefined[0]` will crash the program.
- `The fix` : I just pass "the remote" (Remove the parentheses) `.map(getFirstLetter)`.



## Why is Phase 3 - "The mastery"

Look at execution flow for `badUser`
1. `Maybe(badUser)` -> `Just({id : 1, bio :null})`
2. `.chain(getBio)` -> runs `safeProp('bio')` on the `user`
    - `badUser['bio']` is null
    - `Maybe(null)` returns `Nothing()`
3. `map(getFirstLetter)` -> `Nothing` ignores the map.
    - It does not crash app trying to get [0] of null
    - It simply phases `Nothing` down the line
