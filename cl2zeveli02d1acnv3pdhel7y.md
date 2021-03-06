## How to gradually adopt TypeScript

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
* Multiply two numbers
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

const result = add(2, 5); // add(num1: any, num2: any): any

console.log(result);
```

We can specify the types like this: 

```typescript
// src/utils.js

/**
* Multiply two numbers
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

const result = add(2, 5); // add(num1: number, num2: number): number

console.log(result);

const result2 = add('2', 5); // no error

console.log(result2);
```

Note that at this stage if you provide the wrong types you won't get any errors. This is step 1, which is letting the editor tell us the type of information only.

To go a step further we can add the `@ts-check` comment to the top of the file and this will now do the static type checking and alert us when there is an error. 

```typescript
// src/index.js
// @ts-check
import { multiply } from './utils.js';

const result = multiply(2, 5); // add(num1: number, num2: number): number

console.log(result);

const result2 = add('2', 5); // ??? Argument of type 'string' is not assignable to parameter of type 'number'.

console.log(result2);
```

We can see from this that most of the types are already available through these JSDoc comments. This is JSDoc support in TypeScript. You can convert your JavaScript project into a JSDoc powered TypeScript project first before moving to full TypeScript. This will make moving to TypeScript relatively trivial because all the type information will already be available.

This is fine but what if we want to use more complex types. How do we define those with JSDoc? To illustrate this, let's add another function to our `utils.js` file.

```typescript
// src/utils.js
...

export function getAddress(input) {
  if (!input?.address?.line1) {
    throw new Error("Invalid address");
  }

  return input.address.line1;
}
```

and over in `index.js`:

```typescript
// src/index.js
// @ts-check
import { add, getAddress } from './utils.js';

const result = add(2, 5);

console.log(result);

const firstAddressLine1 = getAddress({ address: { line1: "123 Main Street" } }); // getAddress(input: any): any

console.log({ firstAddressLine1 });

const firstAddressLine2 = getAddress({ name: "Stephen Strange" }); // no error

console.log({ firstAddressLine2 });
```

Notice that we have intentionally included code that should give us an error but JavaScript is not yet smart enough on its own to alert us of this. Second, for us to know what the `getAddress` function expects as valid input we need to look in the source code of `getAddress`.

To add type information to this we add a JSDoc comment again. And for this we can use JSDoc syntax or TypeScript syntax, both will work but typescript syntax is simpler to write, so we'll use that.

```typescript
// src/utils.js
...

/**
 * @param {{ address: { line1: string } }} input
 * @returns {string}
 *
 */
export function getAddress(input) {
  if (!input?.address?.line1) {
    throw new Error("Invalid address");
  }
  
  return input.address.line1;
}
```

What we've done here is to tell TypeScript that this should only accept an object that has a property address which is an object with a line1 property in it that is a string.

---
Here's the JSDoc version of the type in case you're curious: 
```typescript
/**
 * @param {object} input
 * @param {object} input.address
 * @param {string} input.address.line1
 * @returns {string}
 *
 */
```
---

Now that we've added the type, we'll get an error we we gave an input with the wrong type:

```typescript
// src/index.js
// @ts-check
import { add, getAddress } from './utils.js';

const result = add(2, 5);

console.log(result);

const firstAddressLine1 = getAddress({ address: { line1: "123 Main Street" } }); // getAddress(input: { address: { line1: string } }): string

console.log({ firstAddressLine1 });

const firstAddressLine2 = getAddress({ name: "Stephen Strange" }); // ??? 
/* Argument of type '{ name: string; }' is not assignable to parameter of type '{ address: { line1: string; }; }'. Argument of type '{ name: string; }' is not assignable to parameter of type '{ address: { line1: string; }; }'. */

console.log({ firstAddressLine2 });
```

Note that even if the value is supplied from a variable it's still smart enough to know that the structure is wrong:

```typescript
// src/index.js
// @ts-check
import { add, getAddress } from './utils.js';

const result = add(2, 5);

console.log(result);

const firstAddressLine1 = getAddress({ address: { line1: "123 Main Street" } }); // getAddress(input: { address: { line1: string } }): string

console.log({ firstAddressLine1 });

const input = { name: "Stephen Strange" };
const firstAddressLine2 = getAddress(input); // ??? 
/* Argument of type '{ name: string; }' is not assignable to parameter of type '{ address: { line1: string; }; }'. Argument of type '{ name: string; }' is not assignable to parameter of type '{ address: { line1: string; }; }'. */

console.log({ firstAddressLine2 });
```

With reference to what we spoke about concerning structural typing in [part one of this series](https://blog.bhekani.com/typescript-under-the-hood), note that if we provide a type with an address property that is an object with a line1 property that is a string, it is happy even if the object has other members:

```typescript
// src/index.js
// @ts-check
import { add, getAddress } from './utils.js';

const result = add(2, 5);

console.log(result);

const firstAddressLine1 = getAddress({ address: { line1: "123 Main Street" } }); // getAddress(input: { address: { line1: string } }): string

console.log({ firstAddressLine1 });

const input = {
  name: "Stephen Strange",
  address: {
    line1: "123 Main St",
  },
};
const firstAddressLine2 = getAddress(input); // ???

console.log({ firstAddressLine2 });
```

An additional benefit is that when we provide types like this. The code itself is already self-documenting. We no longer have to go into the source of the getAddress function to understand what kind of input it expects or what it will return.

The way we've done it here is the least pedantic that TypeScript can be. If we remove the `@ts-check` comment we stop getting the errors but we still get the developer tooling that typescript provides.

This is a nice place to consider how we would do named types. To show this, let's add another function to our `utils.js` file:

```typescript
// src/utils.js
...

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

The return type of `storeAddress` is compatible with the input type of `getAddress` so it would be helpful if we can define a type that can be passed around between the two. First, let's add the types for the parameters of `storeAddress`:

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
 * @param {string} name
 * @param {string} line1
 * @param {string} city
 * @param {string} postCode
 */
export function storeAddress(name, line1, city, postCode) {
  /*
      function storeAddress(name: string, line1: string, city: string, postCode: string): {
        name: string;
        address: {
          line1: string;
          city: string;
          postCode: string;
        };
      }
  */

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

If we want to create a named type we can do it by creating a JSDoc that's not attached to anything. Then we can specify that new type where we want to use it like this. 

```typescript
// src/utils.js
...

/**
 * @typedef {object} UserInfo an object with address information
 * @property {string} name
 * @property {{ line1: string, city: string, postCode: string }} address
 * 
 */

/**
 * @param {string} name
 * @param {string} line1
 * @param {string} city
 * @param {string} postCode
 * @returns {UserInfo}
 */
export function storeAddress(name, line1, city, postCode) {
	return {
		name, 
		address: {
			line1, 
			city, 
			postCode, 
		}
	}
}

/**
 * @param {UserInfo} input
 * @returns {string}
 *
 */
export function getAddress(input) {
	if (!input?.address?.line1) {
		throw new Error('Invalid address');
	}

	return input.address.line1;
}
```
---

The reason we can do this is that comments are just a black hole that can be used to add many things and compilers that don't support those things will just ignore them.

---

Now our functions are fully typed with a named type that can be passed around. Now we can update the `index.js` file so that everything works:

```typescript
// src/index.js
// @ts-check
import { add, getAddress, storeAddress } from './utils.js';

const result = add(2, 5);

console.log(result);

const input = storeAddress("Stephen Strange", "123 Main St", "London", "ABC123"); // storeAddress(name: string, line1: string, city: string, postCode: string): UserInfo

const addressLine1 = getAddress(input); // getAddress(input: UserInfo): string

console.log({ addressLine1 });
```

Specifying types using JSDoc like this???while powerful???is a little tedious and unfriendly. The next question then is how can we specify types in a friendlier way? We can do this using a `.d.ts` file. 

As usual, let's start by adding another function to `utils.js`:

```typescript
// src/utils
...
export function createUser(name, age, city) {
  return {
    name,
    age,
    city,
  };
}
```

We can now create a new `types.d.ts` file like this:

```typescript
// src/types.d.ts
export interface User {
  name: string;
  age: number;
  city: string;
}
```

And back in `utils.js` we can modify the new `createUser` function like this:

```typescript
// src/utils
...

/**
 * @param {string} name
 * @param {number} age
 * @param {string} city
 * @returns {import('./types').User} User
 *
 */
export function createUser(name, age, city) {
  return {
    name,
    age,
    city,
  };
}
```

Now we can update `index.js` to this:

```typescript
// src/index.js
// @ts-check
import { 
	add, 
	getAddress, 
	storeAddress, 
	addAlbum 
} from './utils.js';

const result = add(2, 5);

console.log(result);

const input = storeAddress(
	'Bhekani', 
	'123 Main St', 
	'London', 
	'ABC123',
);
const addressLine1= getAddress(input);

console.log({ addressLine1 });

const newUser = createUser("Benedict Wong", 50, "London"); // createUser(name: string, age: number, city: string): User

console.log({ newUser });
```

Now if we want to specify the parameters of the function using typescript syntax as well, then we'll need to extract the `createUser` function into its own file so that we can write its own `.d.ts` file. 

```typescript
// src/createUser.js

export function createUser(name, age, city) {
  return {
    name,
    age,
    city,
  };
}
```

Note that we can't just create a `utils.d.ts` file because we would then need to describe the shape of the entire file. So let's do it for `createUser`

```typescript
// src/createUser.d.ts

export function createUser(name: string, age: number, city: string): User;
```

TypeScript prioritises looking at the `.d.ts` files ahead of the `.js` files to figure out the type information. If we wanted to make the definition of `createUser` available globally we would use `declare function ...` instead of `export function ...`. 

The trade-off here with using `.d.ts` files is the loss of co-location of our type definitions. But the benefit is that we get to use the typescript syntax which is more succinct than the JSDoc syntax.

We have achieved all this without having to install the TypeScript compiler. This is because the code editor (VSCode in my case) already has TypeScript support built in. But of course, there's the final step from this, which is to write this as a `.ts` file. This would then need us to add the typescript compiler.

So let's create a new file:

```typescript
// src/addMovie.ts

interface Movie {
	title: string;
	genre: string;
	year: number;
}

export function addMovie(title: string, genre: string, year: number): Movie {
	return { title, genre, year };
}
```

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