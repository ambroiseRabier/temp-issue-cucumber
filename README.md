https://github.com/cucumber/cucumber-js/issues/1373

```shell script
npx create-react-app my-app --template typescript
cd my-app
npm i --save cucumber
npm i --save ts-node
npm i -D @types/cucumber
```
I have added the features folder and it's content, also few npm scripts seen bellow. Nothing more.

---

```
    "cucumber": "cucumber-js --require-module ts-node/register --require features/**/*.ts"
```

```
L:\PROJECTS 2\WEB\temp-issue-cucumber\my-app\features\helpers\steps.ts:1
import { Given, Then, When } from 'cucumber';
       ^

SyntaxError: Unexpected token {
    at Module._compile (internal/modules/cjs/loader.js:721:23)
    at Module.m._compile (L:\PROJECTS 2\WEB\temp-issue-cucumber\my-app\node_modules\ts-node\src\index.ts:858:23)
    at Module._extensions..js (internal/modules/cjs/loader.js:787:10)
    at Object.require.extensions.(anonymous function) [as .ts] (L:\PROJECTS 2\WEB\temp-issue-cucumber\my-app\node_modules\ts-node\src\index.ts:861:12)
    at Module.load (internal/modules/cjs/loader.js:653:32)
    at tryModuleLoad (internal/modules/cjs/loader.js:593:12)
    at Function.Module._load (internal/modules/cjs/loader.js:585:3)
    at Module.require (internal/modules/cjs/loader.js:690:17)
    at require (internal/modules/cjs/helpers.js:25:18)
    at supportCodePaths.forEach.codePath (L:\PROJECTS 2\WEB\temp-issue-cucumber\my-app\node_modules\cucumber\lib\cli\index.js:119:42)
    at Array.forEach (<anonymous>)
    at Cli.getSupportCodeLibrary (L:\PROJECTS 2\WEB\temp-issue-cucumber\my-app\node_modules\cucumber\lib\cli\index.js:119:22)
    at Cli.run (L:\PROJECTS 2\WEB\temp-issue-cucumber\my-app\node_modules\cucumber\lib\cli\index.js:141:37)
```
Typescript not recognized. Until you add the commonjs for module with a tsconfig (see github issue).

---

```
    "cucumberIgnoreFile": "cucumber-js --require-module ts-node/register --require 'features/**/*.ts'"
```

```
> my-app@0.1.0 cucumberWontWork L:\PROJECTS 2\WEB\temp-issue-cucumber\my-app
> cucumber-js --require-module ts-node/register --require 'features/**/*.ts'

UUU

Warnings:

1) Scenario: s1 # features\hello.feature:4
   ? Given the word "hello"
[...]
```
It just ignore the file, and say nothing. It become obvious if you add `throw 'e'` at the start of `steps.ts` file, that is is not read.
Same for:
```
    "cucumberIgnoreFileToo": "cucumber-js --require-module ts-node/register --require 'features/helpers/steps.ts'"
```
Remove the `'` and you'll be fine. https://github.com/cucumber/cucumber-js/blob/master/docs/cli.md#transpilers doc mention
that line with `'`, but that doesn't work.
