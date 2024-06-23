
### `any`
Allows the variable to be of any type without any restrictions.
```ts
let mystery: any = 'a surprise!';

// Reassigning to different types
mystery = 42; // No error, now a number
mystery = false; // Still no error, now a boolean

// Performing operations typical of different types
console.log(mystery.toString()); // Works as a boolean

// Reassigning to an object
mystery = { key: 'value' };
console.log(mystery.key); // No error, accessing property of an object
```

### `unknown`
Is a type-safe counterpart of `any`. It is a type that represents any value, but it is not type-safe to do operations on it.

**When and How to Use `unknown`?:** `unknown` is ideal when you want to ensure type safety, especially when dealing with values from external sources like APIs or user input where the type isn't known upfront or output from a library. It's a way to tell TypeScript that the type needs to be checked before use. It helps in avoiding common mistakes that can occur when dealing with unknown types.

```ts
let uncertainValue: unknown = 'Hello World';

// Trying to use it directly will result in an error
// console.log(uncertainValue.toUpperCase()); // Error

// Type checking
if (typeof uncertainValue === 'string') {
  console.log(uncertainValue.toUpperCase()); // Safe and works!
}
```

In this example, `uncertainValue` is initially of type `unknown`. TypeScript prevents operations like `toUpperCase` until the type is confirmed through a type check (`typeof uncertainValue === 'string'`). This ensures safety in operations.

In this example, we’ll handle an unknown type that could be either a string or an array of numbers:
```ts
let unknownValue: unknown = ["one", 2, "three", 4];

// Directly using unknownValue as an array will cause an error
// console.log(unknownValue.length); // Error

// Type checking for an array
if (Array.isArray(unknownValue)) {
  // Safe to use as an array
  unknownValue.forEach(item => {
    if (typeof item === "string") {
      console.log(`String: ${item}`);
    } else if (typeof item === "number") {
      console.log(`Number: ${item}`);
    }
  });
}
```
In this example, `unknownValue` could be an array, but TypeScript requires us to verify its type first. We use `Array.isArray()` to check if it's an array and then iterate over its elements. For each element, we further check if it's a string or a number before performing any operations. This showcases how `unknown` can be used to ensure type safety in complex structures.

### `never`
Is a type that represents a value that never occurs or it is used to represent the return type of functions that never return a value.
```ts
function throwError(message: string): never {
    throw new Error(message);
}

throwError("Something went wrong"); // This function doesn't return anything
```

### Another use case of `never`
Imagine we have a union type representing different kinds of pets and a function that handles each kind. We’ll use `never` to ensure that every possible pet type is handled:

```ts
// Union type for different kinds of pets
type Pet = 'dog' | 'cat' | 'fish';

// Function to handle different pet types
function handlePet(pet: Pet) {
  switch (pet) {
    case 'dog':
      console.log("Handle a dog");
      break;
    case 'cat':
      console.log("Handle a cat");
      break;
    case 'fish':
      console.log("Handle a fish");
      break;
    default:
      // Ensures that every possible type of Pet is handled
      const exhaustiveCheck: never = pet;
      return exhaustiveCheck;
  }
}

handlePet('dog'); // Outputs: "Handle a dog"
// handlePet('bird'); // TypeScript error: Type '"bird"' is not assignable to type 'Pet'.
```
In this example, `handlePet` is a function that accepts a `Pet` type, which can be either `'dog'`, `'cat'`, or `'fish'`. The `never` type in the `default` case ensures that all cases of Pet are handled. If you try to pass a value like `'bird'` that is not part of the `Pet` type, TypeScript will produce a compile-time error, preventing the code from compiling. This is because `'bird'` is not assignable to the `Pet` type, demonstrating TypeScript's ability to catch such errors early in the development process.

### Sources
- [TypeScript’s any vs unknown vs never: Complete Guide](https://levelup.gitconnected.com/typescripts-any-vs-unknown-vs-never-complete-guide-ac4294408868)