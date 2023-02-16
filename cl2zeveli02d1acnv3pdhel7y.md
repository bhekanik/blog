# How to gradually adopt TypeScript

This article is part two of this series on TypeScript. [Check out part one here](https://blog.bhekani.com/typescript-under-the-hood)

Typescript is great! I think we can all agree on that now. But when you have a massive complex JavaScript project you probably don't want to nuke it all and replace it with TypeScript. The TypeScript errors will likely overwhelm you and you might end up giving up on what would have been the best decision of your project's life.

Fortunately, you don't have to do that because TypeScript is not an all or nothing deal. I know many people often cite that JavaScript is valid TypeScript but that's not what I mean. I'm referring to the fact that you can gradually adopt TypeScript. You can gradually turn it on in your JavaScript files even before you turn them into TypeScript files. That way you can enjoy the benefits of TypeScript even before you make the commitment.

It's more than likely that you're already enjoying some of the benefits of TypeScript in your JavaScript files right now even without doing anything. This is because most modern code editors and IDEs already have great typescript support and use TypeScript to provide some of their features. I work in VSCode so all the references I will make are to it.

Now, let's learn how to incrementally add TypeScript to your JavaScript files.

Say we have the following code

```typescript
// src/utils.js

export function add(num1, num2) {
  return num1 + num2;
}
```

and

```typescript
// src/index.js
import { add } from './utils.js';

const result = add(2, 5);

console.log(result);
```

This currently doesn't really give us much type information and is not type-safe. What is the minimum amount of typing that we can add to get safety? We can use JSDoc.

```typescript
// src/utils

/**
* Add two numbers
*
* @param {*} num1 
* @param {*} num2 
* @returns {*}
*
*/
export function add(num1, num2) {
  return num1 + num2;
}
```

Now if we go to the `index.js` file and hover over the `add` method it will have types already, not very safe but typed.

```typescript
// src/index.js
import { add } from './utils.js';

add(2, 5); // add(num1: any, num2: any): any
```

We can specify the types like this:

```typescript
// src/utils.js

/**
* Add two numbers
*
* @param {number} num1 
* @param {number} num2 
* @returns {number}
*
*/
export function add(num1, num2) {
  return num1 + num2;
}
```

Now if you go to `index.js` and hover on `add` you'll get the type information.

```typescript
// src/index.js
import { add } from './utils.js';

add(2, 5); // add(num1: number, num2: number): number

add('2', 5); // no error
```

Note that at this stage if you provide the wrong types you won't get any errors. This is step 1, which is letting the editor tell us the type of information only.

To go a step further we can add the `@ts-check` comment to the top of the file and this will now do the static type checking and alert us when there is an error.

```typescript
// src/index.js
// @ts-check
import { add } from './utils.js';

add(2, 5); // add(num1: number, num2: number): number

add('2', 5); // ❌ Argument of type 'string' is not assignable to parameter of type 'number'.
```

We can see from this that most of the types are already available through these JSDoc comments. This is JSDoc support in TypeScript. You can convert your JavaScript project into a JSDoc powered TypeScript project first before moving to full TypeScript. This will make moving to TypeScript relatively trivial because all the type information will already be available.

This is fine but what if we want to use more complex types. How do we define those with JSDoc? To illustrate this, let's add another function to our `utils.js` file.

```typescript
// src/utils.js
...

function getProductNumber(product) {
  return product.meta.number;
}
```

and over in `index.js`:

```typescript
// src/index.js
// @ts-check
import { add, getProductNumber } from './utils.js';

add(2, 5);

const productNumber1 = getProductNumber({ 
    meta: { 
        number: 123 
    } 
}); // getProductNumber(product: any): any

const productNumber2 = getProductNumber({ 
    number: 123 
}); // no error
```

Notice that we have intentionally included code that should give us an error but JavaScript is not yet smart enough on its own to alert us of this. Second, for us to know what the `getProductNumber` function expects as valid input we need to look in the source code of `getProductNumber`.

To add type information to this we add a JSDoc comment again. And for this we can use JSDoc syntax or TypeScript syntax, both will work but typescript syntax is simpler to write, so we'll use that.

```typescript
// src/utils.js
...

/**
 *
 * @param {{meta: {number: number}}} product
 */
function getProductNumber(product) {
  return product.meta.number;
}
```

What we've done here is to tell TypeScript that this should only accept an object that has a property address which is an object with a line1 property in it that is a string.

---

Here's the JSDoc version of the type in case you're curious:

```typescript
/**
 * @param {object} product
 * @param {object} product.meta
 * @param {number} product.meta.number
 *
 */
```

---

Now that we've added the type, we'll get an error where we gave an input with the wrong type:

```typescript
// src/index.js
// @ts-check
import { add, getProductNumber } from './utils.js';

add(2, 5);

const productNumber1 = getProductNumber({ 
    meta: { 
        number: 123 
    } 
}); // getProductNumber(product: { meta: { number: number } }): number

const productNumber2 = getProductNumber({ 
    number: 123 
}); // ❌ 
/* Argument of type '{ number: number; }' is not assignable to parameter of type '{ meta: { number: number; }; }'.
  Object literal may only specify known properties, and 'number' does not exist in type '{ meta: { number: number; }; }' */
```

Note that even if the value is supplied from a variable it's still smart enough to know that the structure is wrong:

```typescript
// src/index.js
// @ts-check
import { add, getProductNumber } from './utils.js';

add(2, 5);

const product = { 
    meta: { 
        number: 123 
    } 
}

const productNumber1 = getProductNumber(product); // getProductNumber(product: { meta: { number: number } }): number

const notProduct = { 
    number: 123 
}

const productNumber2 = getProductNumber(notProduct); // ❌ 
/* Argument of type '{ number: number; }' is not assignable to parameter of type '{ meta: { number: number; }; }'.
  Object literal may only specify known properties, and 'number' does not exist in type '{ meta: { number: number; }; }' */
```

With reference to what we spoke about concerning structural typing in [part one of this series](https://blog.bhekani.com/typescript-under-the-hood), note that if we provide a type with an address property that is an object with a line1 property that is a string, it is happy even if the object has other members:

```typescript
// src/index.js
// @ts-check
import { add, getProductNumber } from './utils.js';

add(2, 5);

const product = { 
    meta: { 
        number: 123 
        mass: 100
    }, 
    name: "Product1"
}

const productNumber1 = getProductNumber(product); // getProductNumber(product: { meta: { number: number } }): number

const notProduct = { 
    number: 123
}

const productNumber2 = getProductNumber(notProduct); // ❌ 
/* Argument of type '{ number: number; }' is not assignable to parameter of type '{ meta: { number: number; }; }'.
  Object literal may only specify known properties, and 'number' does not exist in type '{ meta: { number: number; }; }' */
```

An additional benefit is that when we provide types like this. The code itself is already self-documenting. We no longer have to go into the source of the `getProductNumber` function to understand what kind of input it expects or what it will return.

The way we've done it here is the least pedantic that TypeScript can be. If we remove the `@ts-check` comment we stop getting the errors but we still get the developer tooling that typescript provides.

This is a nice place to consider how we would do named types. To show this, let's add another function to our `utils.js` file:

```typescript
// src/utils.js
...

function createProduct(name, no, mass) {
  return {
    meta: {
      number: no,
      mass,
    },
    name,
  };
}
```

The return type of `createProduct` is compatible with the input type of `getProductNumber` so it would be helpful if we can define a type that can be passed around between the two. First, let's add the types for the parameters of `storeAddress`:

```typescript
// src/utils.js
...

/**
 * @param {string} name
 * @param {string} line1
 * @param {string} city
 * @param {string} postCode
 */
export function storeAddress(name, line1, city, postCode) {
  return {
    name,
    address: {
      line1,
      city,
      postCode,
    },
  };
}
```

With this, Typescript can already infer the return type of `storeAddress`, you can see this by hovering on `storeAddress`.

```typescript
// src/utils.js
...

/**
 *
 * @param {string} name
 * @param {number} no
 * @param {number} mass
 */
function createProduct(name, no, mass) {
  /*
      function createProduct(name: string, no: number, mass: number): { meta: { number: number; mass: number; }; name: string; }    
  */
return {
    meta: {
      number: no,
      mass,
    },
    name,
  };
}
```

If we want to create a named type we can do it by creating a JSDoc that's not attached to anything. Then we can specify that new type where we want to use it like this.

```typescript
// src/utils.js
...

/**
 * @typedef {{meta: {number: number}}} Product
 */

/**
 * @param {Product} product
 */
function getProductNumber(product) {
  return product.meta.number;
}

/**
 *
 * @param {string} name
 * @param {number} no
 * @param {number} mass
 * @returns {Product}
 */
function createProduct(name, no, mass) {
  // ^?
  return {
    meta: {
      number: no,
      mass, /* ❌ Type '{ number: number; mass: number; }' is not assignable to type '{ number: number; }'.
  Object literal may only specify known properties, and 'mass' does not exist in type '{ number: number; }' */
    },
    name,
  };
}
```

But notice that we now get an error for the non-specified properties. So we need to be explicit about them and say that they are optional.

```typescript
// src/utils.js
...

/**
 * @typedef {{meta: {number: number, mass?: number}, name: string}} Product
 */

/**
 * @param {Product} product
 */
function getProductNumber(product) {
  return product.meta.number;
}

/**
 *
 * @param {string} name
 * @param {number} no
 * @param {number} mass
 * @returns {Product}
 */
function createProduct(name, no, mass) {
  // ^?
  return {
    meta: {
      number: no,
      mass, 
    },
    name,
  };
}
```

---

The reason we can do this is that comments are just a black hole that can be used to add many things and compilers that don't support those things will just ignore them.

---

Now our functions are fully typed with a named type that can be passed around. Now we can update the `index.js` file so that everything works:

```typescript
// src/index.js
// @ts-check
import { add, getProductNumber } from './utils.js';

add(2, 5);

const product = { 
    meta: { 
        number: 123 
        mass: 100
    }, 
    name: "Product1"
}

const productNumber1 = getProductNumber(product); // getProductNumber(product: { meta: { number: number } }): number

const notProduct = { 
    number: 123
}

const productNumber2 = getProductNumber(notProduct); // ❌ 
/* Argument of type '{ number: number; }' is not assignable to parameter of type 'Product'.
  Type '{ number: number; }' is missing the following properties from type 'Product': meta, name */
```

Even the error for `productNumber2` is updated to use our new named type.

Specifying types using JSDoc like this—while powerful—is a little tedious and unfriendly. The next question then is how can we specify types in a friendlier way? We can do this using a `.d.ts` file.

We can now create a new `types.d.ts` file like this:

```typescript
// src/types.d.ts
export type Product = {
  name: string;
  meta: {
    number: number;
    mass: number;
  };
};
```

And back in `utils.js` we can modify the functions like this:

```typescript
// src/utils
...

/**
 * @typedef {import('./index').Product} Product
 */

/**
 * @param {Product} product
 */
function getProductNumber(product) {
  return product.meta.number;
}

/**
 *
 * @param {string} name
 * @param {number} no
 * @param {number} mass
 * @returns {Product}
 */
function createProduct(name, no, mass) {
  // ^?
  return {
    meta: {
      number: no,
      mass, 
    },
    name,
  };
}
```

Now if we want to specify the parameters of the function using typescript syntax as well, then we'll need to a `.d.ts` file for utils.

```typescript
// src/utils.d.ts

export function getProductNumber(product: Product): number;

export function createProduct(name: string, no: number, mass: number): Product;
```

TypeScript prioritises looking at the `.d.ts` files ahead of the `.js` files to figure out the type information. If we wanted to make the definition of `createUser` available globally we would use `declare function ...` instead of `export function ...`.

The trade-off here with using `.d.ts` files is the loss of co-location of our type definitions. But the benefit is that we get to use the typescript syntax which is more succinct than the JSDoc syntax.

We have achieved all this without having to install the TypeScript compiler. This is because the code editor (VSCode in my case) already has TypeScript support built in. But of course, there's the final step from this, which is to write this as a `.ts` file. This would then need us to add the typescript compiler.

This doesn't immediately work. We can use `ts-node` to run it or we can install the typescript compiler by running `npm install --save-dev typescript`. Next, run `npm init -y` then in the `package.json` file, add the following build script:

```json
// package.json
...
"scripts": {
	"build": "tsc",
	...
}
...
```

Now we need a `tsconfig.json` file. We can auto-generate this with `npx tsc --init`. After this, if you run `npm run build` you should get a `.js` file next to the `.ts` file. You still have all the information in your `.ts` file and everything is co-located. But you also have everything working with the `.js` files.

And that's it, we've incrementally added TypeScript to our little project. Managing to go a big part of the way before we had to install the TypeScript compiler.

A key takeaway is that migration to TypeScript can be gradual, so team members can learn the new syntax on the job, and you can start to get a feel of the real benefits before fully committing. If you have a massive project that's in motion, you don't have to make everyone drop tools and start converting things to typescript. You can even get the type checking on files and projects that you never intend to convert to typescript.

I hope this convinces some of the remaining holdouts out there. Or helps anyone who can't convert their project fully to TypeScript for whatever reason.

Edit: I wrote this as a way to move into Typescript but I have since learnt that others are using these approaches to move away from Typescript to improve their build times:

https://twitter.com/JSPartyFM/status/1534170467396816896?t=papevQ1rRGxOpFJK1f9U1A&s=19

You get the types for free without paying the penalty in build times.