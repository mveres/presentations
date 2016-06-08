#Immutability

There are no _variables_, everything is a _value_.

`const a = 42;`

In functional programming everything is immutable by default.

My personal rule: If I need to use a mutable variable I have to rethink it.

Safer code with immutability - you can’t screw up what you can’t change.

```javascript
const square = n => n * n;
const sum = (a, b) => a + b;

const sumOfSquaresImperative = numbers => {
  let total;
  for (let i = 0; i < numbers.length; i++) {
    total += square(n);
  }
};

const sumOfSquaresFunctional = numbers =>
  numbers.map(square).reduce(sum);
```

Sneak peak: `let sumOfSquares = List.map square >> List.sum`


#Functions as values

###Most programming languages -> functions and data are regarded as different things

###Functional programming languages -> functions are treated like any other data


Functions can:
- take other functions as parameters
- create and return new functions

These are called Higher Order functions.
Ex: map, filter, reduce


#Currying

- after Haskell Curry - mathematician with important influence on functional programming
- ... partial function application
- ... a mathematical function can only have one parameter -> a function with multiple parameters is rewritten as a series of new functions, each with only one parameter

```javascript
const sumNotCurried = (a, b) => a + b;
const sumCurried = a => b => a + b;

const s1 = sumNotCurried(1, 2);
const s2 = sumCurried(1)(2);

const add1ToEach = numbers => numbers.map(e => sumNotCurried(e, 1));

const add2ToEach = numbers => numbers.map(sumCurried(2));

const addNToEach = n => numbers => numbers.map(sumCurried(n));
addNToEach(n)(numbers);
```

#Pipes

Chain everything!!!

`|>` - pipe forward operator
Passes the result of the left side to the function on the right side (forward pipe operator).

```
[1..10]
|> List.map square
|> List.filter ((>) 50)
|> List.sum
|> (printf "%A")
```

`<|` - pipe backward operator - the less used brother

#Closures

- a value that is captured by the scope of a function - it closes over the value

```javascript
const i = 7;
const multiply = n => n * i;
```

#Pattern matching

- similar to `switch` statements but much more powerful



```
let rec somethingWithAList l =
    match l with
    | [] -> "it is empty"
    | [42] -> "the answer to everything!!!"
    | [_; 42; _] -> "the answer is in the middle"
    | h :: t -> "the answer is out there" + somethingWithAList t
```

- destructuring in javascript

```javascript
const { id, name, ...everything } = person;
const [first, second, rest] = persons;
```

- hidden in `try-catch` statements in C#, Java, ...
```
try {
  throw new InterestingException();
}
catch (VeryInterestingException viex) {
  doSomethingWithIt(viex);
}
catch (InterestingException iex) {
  doSomethingElseWithIt(iex);
}
catch (Exception ex) {
  dealWithIt(ex);
}
```

#Discriminated Unions

- a.k.a. algebraic data types

```
type Tree =
    | Empty
    | Leaf of int
    | Node of Tree * Tree

match t with
| Empty -> "there's nothing here"
| Leaf (value) -> "there's ${value} in the leaf"
| _ -> "not sure what to do with this"

type Optional<T> = None | Some of T


Java:

String strNull = null;
Optional nullableOptional = Optional.ofNullable( strNull );
Optional sizeOptional = stringOptional.map( String::length );

```

#Cons Cells (Singly Linked List)

- an algebraic data type
```
data List a = Nil | Cons a (List a)
Cons 1 (Cons 2 (Cons 3 Nil))

1::2::3::[]
head::tail
```

#Recursion & Tail-Recursion

- recursion + immutability = something
- tail recursive patterns: accumulator, continuation
- tail call optimization - javascript has it (ES6)

```javascript
const isEven = x => x % 2 === 0;

const filter = (xs, isOk) => {
  if (!xs.length) return [];
  const [head, ...tail] = xs;

  const newHead = isOk(head) ? [head] : [];
  const newTail = filter(tail, isOk);
  return [...newHead, ...newTail];
};

console.log(filter([1, 3, 6, 8], isEven));


const tailRecursiveFilter = (xs, isOk, acc) => {
  if (!xs.length) return acc || [];
  const [head, ...tail] = xs;

  const newAcc = isOk(head) ? [...acc, head] : acc;
  return filter(tail, isOk, newAcc);
};

console.log(tailRecursiveFilter([1, 3, 6, 8], isEven));
```

##TBD: side-effects, monads, reactive programming

###Further Reading/Learning
- Programing F# by Chris Smith
- Functional Programming Principles in Scala by Martin Odersky (https://www.coursera.org/learn/progfun1)
- Introduction to Functional Programming by Erik Meijer (https://www.edx.org/course/introduction-functional-programming-delftx-fp101x-0)
- https://fsharpforfunandprofit.com/
