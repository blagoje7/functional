In imperative programming you write procedures (step 1, step 2, step 3), in functional programming you build pipelines.
To build pipelines, you need specific you need tools that dont really exist in imperative programming. 
Currying and Composition.

Tool 1:
Currying ("Configuration tool")
    -> The problem : Most functions in JS take in multiple arguments at once. This makes them hard to combine.

        //Imperative style
        const add = (a, b) => a + b;
        // You MUST have both arguments ready at the same time to call the function.

    -> The solution : Currying transforms a function so it takes arguments One at a time. It lets you confgure the function now
        and call it later.

        //Curried version:
        //Type: Number -> Number -> Number
        const add = x => y => x + y;

        const addFive = add(5);

        //... 100+ lines of code

        console.log(addFive(10));

    -> Why is this a weapon : It lets us creating specialized function from generic ones without rewriting the logic.

        //Generic: 
        checkPermission(role, user);

        //Specialized:
        const isAdmin = checkPermission('admin') // now you can pass isAdmin to a fliter function easily.

Tool 2:
Composition ("The pipeline tool")
    -> The Math: (f o g)(x) = f(g(x))

    -> The problem : In imperative code we nest calls like onions, its very hard to read inside out.

    //Not easy to read
    const result = toUpperCase(trim(getName(user)));

    -> The solution (Composition) : Lets us stick functions together like pipes. The output of one becomes the input of next.

        //A simple composition function right to left Execution
        const compose = (f , g) => x => f(g(x));

        //Usage
        const getName = name => user.name;
        const upperCase = str => str.toUpperCase();

        //We build new function by sticking two others together
        const getUpperName = compose(upperCase, getName);

        console.log(getUpperName({name : "haskell" }));


Rules of arrow function parentheses:
    o The Rules of Arrow Function Parentheses:

        -> 0 Arguments: Parentheses are mandatory. const sayHi = () => console.log('Hi');

        -> 2+ Arguments: Parentheses are mandatory. const add = (a, b) => a + b;

        -> 1 Argument: Parentheses are optional. x => x * 2 is the same as (x) => x * 2
