---
layout: post
title:  "Setup jest for typescript project"
date:   2022-08-06 20:45:39 -0700
categories: typescript yarn yarn3 jest ts-jest test coverage
---

Unit tests are integral to successful code implementations. Today jest is by far the most popular in the nodejs and typescript world. Let us setup jest for our typescript project.

>All sources for this example can be referenced here in *[github](https://github.com/rabiddroid/example-typescript-template)*.
>

## Install and configure jest

Run command
```bash
yarn add jest @types/jest ts-jest --dev
```
_ts-jest is jest transformer that enables jest to understand typeScript._

Initialize your test configuration by:
```bash
yarn ts-jest config:init
```
_This creates jest.config.js file that specify ts-jest as the preprocessor for Jest_


The jest.config.js file contents may look like this.
```js
/** @type {import('ts-jest/dist/types').InitialOptionsTsJest} */
module.exports = {
  preset: 'ts-jest',
  testEnvironment: 'node'
};
```


> See [Jest Configuration Options](https://jestjs.io/docs/configuration) for further information about configuring jest.

---
<details>
  <summary markdown="span">Optional!! Going all in with typescript.</summary>
If you choose to, you can convert your jest.config.js file to jest.config.ts file and stay true by keeping everything typescript.


Install types for ts-jest
```bash
yarn add @jest/types --dev
```

Rename the file as jest.config.ts and replace the contents with
```
import type { Config } from "@jest/types";

export default async (): Promise<Config.InitialOptions> => {
  return {
    preset: 'ts-jest',
    testEnvironment: 'node'
  };
};

```
</details>

---

<br>
<br>

## Running a test

Now that we have configured our test environment. Let us add a test.

In the spirit of TDD, we shall actually first write our test before writing the code.

Update scripts in package.json. Add the following to scripts.
```json
"scripts": {
    "test": "jest"
  }
```
_This allows us to run jest as a yarn command._


Under project root dir, create a file under src in the following manner
```
./src/__test__/actions.test.ts
```
>_I prefer to keep my tests close to my code. You can chose to keep your tests under a separate root folder e.g \<projectRoot\>/test/..path matching target source file._


Add the following code to your test file actions.test.ts
```
describe('validate action methods', () => {
    it('returns valid greetings', () => {

    })
});
```

Run in project root folder
```bash
yarn test
```
You should see a test passing result like:
```bash
$ yarn test
 PASS   example-typescript-project  src/__test__/actions.test.ts
  validate action methods
    ✓ returns valid greetings (1 ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        0.456 s, estimated 1 s
Ran all test suites.
```

### _Getting the test to fail_

Update the test code as:
```ts
import { greeting } from "../actions";

describe('validate action methods', () => {

    it('returns valid greetings', () => {

        expect(greeting()).toEqual('hello world!');

    })

});
```

Create a new file under project root as
```bash
src/actions.ts
```
Save the following code:
```ts
export function greeting(): string{
    return '';
}

```

Now run
```bash
yarn test
```
You will see test results like below:
```bash
$ yarn test
 FAIL   example-typescript-project  src/__test__/actions.test.ts
  validate action methods
    ✕ returns valid greetings (2 ms)

  ● validate action methods › returns valid greetings

    expect(received).toEqual(expected) // deep equality

    Expected: "hello world!"
    Received: ""

       6 |     it('returns valid greetings', () => {
       7 |
    >  8 |         expect(greeting()).toEqual('hello world!');
         |                            ^
       9 |         
      10 |
      11 |     })

      at Object.<anonymous> (__test__/actions.test.ts:8:28)

Test Suites: 1 failed, 1 total
Tests:       1 failed, 1 total
Snapshots:   0 total
Time:        0.505 s, estimated 1 s
Ran all test suites.
```
> This is great! 😁
>
>It means our setup works and testing is happening successfully.

_Now lets fix our code for passing tests._

Update actions.ts
```ts
export function greeting(): string{
    return 'hello world!';
}

```
Running `yarn test` yields
```bash
$ yarn test
 PASS   example-typescript-project  src/__test__/actions.test.ts
  validate action methods
    ✓ returns valid greetings (1 ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        0.501 s, estimated 1 s
Ran all test suites.
```

---
**Bonus**
> _Enable code coverage reports_
>
> In jest.config.(js|ts), add entry `collectCoverage: true`.
>
>You will now get code coverage reports when running `yarn test`.
---

🙌
<p>Great job! Now you are one step closer to writing quality code.
