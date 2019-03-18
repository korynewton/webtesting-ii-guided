# Web Testing I Guided Project

Guided project for **Web Testing I** module.

In this module we will cover the basics of automated testing and use `jest` to write unit tests.

## Project Setup

- [ ] fork and clone this repository.
- [ ] **CD into the folder** where you cloned **your fork**.

Please follow along as the instructor uses _Test Driven Development (TDD)_ to implement the `add()` feature of a simple `calculator`.

## Starter Code

The [Starter Code is here](https://github.com/LambdaSchool/webtesting-i-guided). Ask students to fork it so they can do commits to their fork as they follow along.

Fork the starter code yourself if you want to demo making frequent commits.

## Introduce the Module Challenge

Take time to explain what is expected from the [module challenge](https://github.com/LambdaSchool/webtesting-i-challenge), and provide hints about what to test.

## Introduce Testing

Open TK and go through the content, emphasis on:

- what is automated testing.
- why should we use automated testing.
- what `refactoring` and `regressions` are.
- type of automated tests.

**time for a break? Take 5 minutes\***

Introduce the [guided project](https://github.com/LambdaSchool/webtesting-i-guided) and make sure all students have it forked and cloned.

## Add Jest

- introduce `jest`, show the [docs](https://jestjs.io/docs/en/getting-started.html)
- add `jest` as a dev dependency.
- add test script: `jest --watch`, explain `--watch` flag.
- type `yarn test` to start the test runner. Explain that `jest` uses `git` to know if the JavaScript files have changed in order to run the tests.
- how does `jest` find tests to run?
  - add `__tests__` folder with an `index.js` file inside. Note that `jest` looks for tests inside it, explain why. Note the message explaining that a `suite` (a file) must have test or it will fail the test run.
  - add an `index.test.js` to root. Explain why `jest` tries to run tests from this file.
  - add an `index.spec.js` to root. Explain why `jest` tries to run tests from this file.

We will write tests only inside `./calculator/calculator.spec.js`, so go ahead and remove the `__tests__` folder, `index.test.js` and `index.spec.js` as they are not needed and will only cause noise in the terminal. **Make sure students know that yo manually removed them.**

**wait for students to catch up**

We haven't written any tests yet, we'll do that next.

## Add First Test

- inside `./calculator/calculator.spec.js`:

```js
// this test is useful to confirm the tooling is working
test('runs the tests', () => {
  // explain jest globals, it and test (show the docs for globals)
  expect(true).toBe(true); // explain assertions and matchers
});
```

**wait for students to catch up**

Time to start implementing the calculator, but we'll build it using the `TDD workflow`.

## Introduce TDD

- find an [image that depicts the TDD workflow](https://centricconsulting.com/wp-content/uploads/2018/07/TDD1.jpg), and show it as you explain how TDD works.

## Test calculator.js

- add a test for the `add()` method.
  - _should return the sum of 2 numbers_.

```js
const { add } = require('./calculator.js');

// introduce the describe global
describe('calculator.js', () => {
  // other tests

  // note that we  can nest describes
  describe('add()', () => {
    // introduce the it() global as an alternative for the test() global
    it('should return the sum of numbers passed', () => {
      expect(add(2, 2)).toBe(4); // could be a false positive
    });
  });
});

// calculator.js
function add(a, b) {
  return a * b; // multiplication and test passes
}
```

Note that the code is wrong, but the test passes. Explain that we can have multiple assertions in the same test.

Add another assertion.

```js
it('should return the sum of two numbers', () => {
  expect(add(2, 2)).toBe(4); // could be a false positive
  expect(add(2, 3)).toBe(5); // helps reveal the bug
});
```

Fix the code to return the sum: `return a + b;`

**wait for students to catch up**

## Introduce test.todo() and it.todo()

As we write one test, ideas for other tests come to mind we can write placeholders for those tests.

```js
// remember to leave out the function
it.todo('should return 0 when called with no arguments'); // show the terminal output for todo tests
```

## Introduce AAA

Explain how thinking about our test in the context of `Arrange > Act > Assert` can help when writing tests. An example:

```js
it('should return 0 when called with no arguments', () => {
  // arrange
  const expected = 0;

  // act
  const actual = add();

  // assert
  expect(actual).toBe(expected);
});
```

Use _default function parameters_ to make it pass.

```js
// default function parameters to the rescue
function add(a = 0, b = 0) {
  return 0;
}
```

Some tests read better organized this way, others, like this test, can be written in a more compact manner.

```js
// format the test to be more compact
it('should return 0 when called with no arguments', () => {
  expect(add()).toBe(0);
});
```

**wait for students to catch up**

## You Do (estimated 5 mins to complete)

Write a test for: _should return the value passed when only one argument is provided_.

One possible solution:

```js
it('should return the passed in value when only one argument provided', () => {
  expect(add(2)).toBe(1); // we do this to make sure the test can fail
  expect(add(-1)).toBe(-1);
});
```

Fix the test to make it pass: `expect(add(2)).toBe(2);`

**wait for students to catch up**

**take a break if needed**

New feature: _add support for any number of arguments_.

```js
it('should handle any number of arguments separated by comma', () => {
  expect(add(1, 2, 3)).toBe(6);
  expect(add(1, 2, 3, 5)).toBe(11);
});

// calculator.js
function add(args) {
  return Array.from(arguments).reduce((sum, value) => {
    return sum + value;
  }, 0);
}
```

**wait for students to catch up**

New feature: _add support for accepting an array of numbers_.

```js
it('should handle an array of numbers', () => {
  expect(add([1, 2, 3])).toBe(6);
  expect(add([1, 2, 3, 5])).toBe(11);
});

// calculator.js
function add(args) {
  const values = Array.isArray(args) ? args : Array.from(arguments);

  return values.reduce((sum, value) => {
    return sum + value;
  }, 0);
}
```

Explain how this is similar to most agile environments. New features are requested and we need to implement them without breaking existing functionality. Having a test suite provides **confidence**.

**wait for students to catch up**

Open the [jest documentation for expect](https://jestjs.io/docs/en/expect) and show some of the matchers provided by `jest`.
