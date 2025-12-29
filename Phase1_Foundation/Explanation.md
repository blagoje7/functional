Function in JS is not same as mathematical function. Because of scope and mutation.
Function should be "anonymous" and operate only on its inputs.

#
example 1: badImperative.html
# Why is imperative Bad ? 

The core issue is that it forces you to manage memory and time in your head, something which humans are really bad at.

# 1. Shared muttable state.
> In imperative programming variables are containers that change over time.
o `The problem` : If a variable x is modified by function A and function B also uses x. Result of function B is entirely dependant on when is function B called relative to function A.
o `The trap` : Non-determinism, you cannot test function B in isolation
because its output changes based on history of application not just its inputs.
# 2. Side effects 
> Function's signature should be a contract : "Give me X, I give Y"
o `The problem` : In imperative code functions often do secret work, writing to a database, changing DOM, logging to the console...
`These are side effects` If a function have `side effects`, you cannot predict what exactly will happen after calling it just by reading the code. You have to read every single line.
o `Concept` : This breaks Referential Transparency ( ability to replace a function call with just its result without breaking the program )
# 3. Complexity overhead
o `Imperative` : describes "how" to do something ( step by step instructions "Iterate from 0 to 10; check if even; push to array )
o `Functional` : describes "what" something is ( declarative "The result is a filtered list of evens" )
> `Trap` : Imperative code hides the intent behind the Implementation. You spend mental energy tracing loops instead of understanding logic.

# Most common traps in JS.

# 1. The reference mutation.
o in JS objects are passed by reference 
> `The pit` : You pass an array to the function, modify it inside and do not realize you just broke the data for rest of the app.
```javascript
        # Bad js. 
        const sortNumbers = (arr) => {
            return arr.sort();      //sort mutates original array in place.
        }

        # Fix
        const sortNumbers = (arr) => {
            return [...arr].sort();     //sort a copy. safe
        }
```

# 2. The hidden input 
o Function accessing anything but its arguments
- `The pit` : using this, window or global let variable
- `Why is it a trap` : Your function is no longer portable, you cannot copy paste it into another file because it uses invisible context.
- `The rule` : If its not into ( arguments ) it doesnt exist.

3. Time dependant functions
o Functions that return different results at different times even with the same inputs.
- `The pit` : Using Date.now() or Math.random() directly inside your logic.
- `Why is it a trap` : You cannot unit test this code easily. "If tested on Tuesday it fails, on Wednesday it passes."
- `Fix` : Pass time or randomness as argument. ( Dependency injection )

4. The Loop variable leak
o Using var ( sometimes let ) in loops where scope leaks out or persists longer then expected.
- `The pit` : The value i after the loop is still accessible and can affect late code.



# `example 2`: goodFunctional.html

When passing value + 1 in recursion is new variable being made ?

> Short answer: Yes.

# The mechanics:
In JavaScript ( and most languages ) every time you call a function, engine creates a new Execution Context ( a fresh box of memory ).
- `Scope` : Variables times and value inside the function are local to that specific call
- `What happens` :
    1. JS calculates value + 1, eg. 0 + 1 = 1
    2. It crates new Execution Context for next call
    3. It places 1 into the new context as a argument 
- `The stack` : If this process repeats 10 times we have 10 separate boxes each with its own "value" as variable.
- `Why it matters` : This recursion can cause stack overflow, ie. you run out of memory for these "boxes".
        
> Loops do not do this, they reuse same scope. Some languages optimize this away with `"Tail Call Optimization"` but standard JS does not do this yet, although it is part of ES6 spec.

