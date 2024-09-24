---
title: The pitfall of class property initialization
date: 2024-09-24 17:32:50
tags:
    - TypeScript
    - ECMAScript
    - ES2022
---

## Introduction
The implementation of class property initialization in JavaScript and TypeScript are diverged from some point.

**TypeScript** move the property initialization into the constructor.
**ES2022** initialize the property before the constructor is called.

## Examples

1. Simple explanation
    ```ts
    // -- ES2022 --
    class User {
      age = 25;
    }
    // -- ES2022 --

    // -- TypeScript --
    // TRANSPILED CODE:
    class User {
      constructor() {
        // The property initialization is moved into the constructor
        this.age = 25;
      }
    }
    // -- TypeScript --
    ```

2. Compute property based on constructor parameters
    ```ts
    // -- ES2022 --
    class MyComponent {
      // this.myService could be undefined!
      data = this.myService.getData();
      constructor(private myService: Service) {}
    }
    // -- ES2022 --

    // -- TypeScript --
    // TRANSPILED CODE:
    class MyComponent {
      constructor(myService) {
        this.myService = myService;
        // this.data is initialized correctly
        this.data = this.myService.getData();
      }
    }
    // -- TypeScript --
    ```

3. Inheritance
    ```ts
    class Parent {
      pName: string;
      constructor(name: string) {
        this.pName = name;
      }
    }
    class Child extends Parent {
      pName: string;
      constructor(name: string) {
        super(name);
      }
    }
    
    const p = new Child('dog');
    console.log(p.pName);

    // ES2022 => `undefined`
    // TypeScript => 'dog'
    ```

    If you run the above code in browser, you could debug the execution order: 
    `child.constructor` => "`parent.pName` initialization", `parent.constructor`, "`child.pName` initialization"

    **Conclusion**:
    1. Class properties are always defined on the instance object itself, not on the prototype chain.
    > So the `Child` class 's `pName` is `undefined`, it will not look up the `pName` property on prototype chain.

    2. When `super()` is called, `this` refers to the subclass instance, and the entire process does not create a parent class object. The prototype chain is: subclass instance -> subclass prototype -> parent class prototype.
    > So you can't find the `pName = 'dog'` on the prototype chain, because it only exists on `Parent` instance. And actually initialization of `pName` in the `Parent` was applied to the Child instance.

    3. The initialization of instance properties occurs before the constructor, but after the parent class constructor (i.e., the `super()` call), because the instance is created when `super` starts executing.
    > So the `Child` class 's 'pName' was initialized to `undefined` after `super(name)`

    4. If the subclass defines the same member property as the parent class, it will be initialized twice.
    > Initialized in `Parent` and `Child`


## Solutions
1. Move all your property initialization into the constructor.
2. Use target: `es2022` and enable `useDefineForClassFields` in your `tsconfig.json` to make TypeScript to behave like ES2022.
3. Disable `useDefineForClassFields` in your `tsconfig.json` to adopt TypeScript behavior (property initialization in the constructor).