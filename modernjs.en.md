# Modern JavaScript overview, and to Electron

I made a repository that organized the learning outcomes in the last one month so that I summarized the results here.

If you want the sample project I just created immediately, please clone this repository.

* [taichi/js-boilerplate](https://github.com/taichi/js-boilerplate)
  * The master branch contains a minimal JavaScript development environment with sample code
  * The frontend branch contains development environments for [React] / [Redux] / [webpack] web applications
  * In the electron branch, the default branch, contains an environment for [Electron] application development in addition to the contents of the frontend branch

# Introduction

## About Modern JavaScript
I'm not writing JavaScript as a job, but in the last six months, I have made some small tools. I believe both of them are useful, so please try them and it would be great if you like them.

* [ci-yarn-upgrade](https://github.com/taichi/ci-yarn-upgrade)
  * A tool that automatically creates PullRequest if there is a dependency library update in a project using [Yarn]
* [vscode-textlint](https://github.com/taichi/vscode-textlint)
  * [textlint](https://github.com/textlint/textlint) extension for [VS Code]

Well, as you see, though I am not following the latest JavaScript on daily basis, I know some.

My opinion is that what the developers, at least I, want to do and can do with JavaScript haven't changed much in recent JavaScript ecosystem, and the scope hasn't got larger then you think. You may feel there have been a lot of changes there, but the truth is that a variety of libraries just pop up and dissapears.

The volume of the content here is kind of large, since I picked up as much movement as possible in the last twot to three years. Though I just told you that there haven't been much changes, three years is long enough for the technology changes. 

## Main problems in JavaScript

From now on, I'm going to explain the knowledge about JavaScript I recently learned, and I listed the areas to be discussed in advance.

This is the list of "What I wanted to do and I am able to do" mentioned earlier.

* Topics on the Language
  * Introduction of type system
  * Topics on static analysis
* Topics on Testing
* Topics on Advanced UI
* Topics on Building project
  * Management of dependent modules
  * Module bundle
* Topics on Electron

## Target audience

The target audience of this document is supporsed to be programmers who have first languages that they can use them fluently. The languages are preferably those with compiling process such as Java and C#.

Also, those who are interested in new JavaScript and those who want a document to be a guide for effective learning.

Thus, this entry is not aimed at those who can not write much code or those who are not interested in JavaScript.

Advanced JavaScript developers are not subject readers, but it is nice if you point out something strange or something misleading. It would be more than greatful if you can tell more accurate information via [Twitter](https://twitter.com/ryushi).

## Development environment
I use Windows 10, just in case.

Since there are few platform dependencies in this story, no matter what OS you use, it does not matter; just for your information.

I recommend to install Node using [nvm-windows](https://github.com/coreybutler/nvm-windows).
I like it because it causes less trouble than [nodist](https://github.com/marcelklehr/nodist).
For Mac, I think that it is good to use [nvm](https://github.com/creationix/nvm). Well, I'm not a serious Mac user, so feel free to use others if you have more preferable tools.

After installing Node, install [Yarn](https://yarnpkg.com/) as well. Even if you do not know what this is, no problem. I'll explain it later.

As for editor, I built the environment on the premise of [VS Code].
However, since I did not use extensions which only run on [VS Code], it is not difficult to reproduce the similar environment with other editors.

The `.vscode/extensions.json` directly under the project has the ID list of the [VS Code] extensions.
Open the directory with [VS Code] where you did `git clone` the repository and approve the confirmation dialog, and then all required extensions will be installed automatically.

I use Chrome as a browser to check the application behavior.

### Hardware
The spec of my laptop is as follows:

* Thinkpad X1 Carbon 2016 model
* CPU i 7 6600 U @ 2.6 GHz
* Memory 16 GB
* Storage SAMSUNG NVMe SSD 950 PRO 512 GB

It may be a little expensive, but it's usual for developers, isn't it?

## My engineering background
I started my career from a place where I spent about ten years as "a soldier" who makes enterprise systems in Java with a contracted system with SIer.
So, I have a lot of knowledge about Java, and I know most about server side technology in Java.

Though now I can write programs with various languages, still those outputs, like Ruby and Python, are the result of the conversion from the strange, unique, my own language closely similar to Java that only exists in my brain.

With that nature, I have a tendency to prefer the environment where I can easily convert the target language to the Jave like language when I learn new languages other than Java.

In addition to that, since I have established my career in and I am still working for SIers, I am keenly interested in how to use the technologies in contract development system developments and package system developments.

In other words, I tend to think about how to apply new technologies to those development projects on the premise that those technologies and the improvements of those are seldom tied to the revenue in those projects.

Please understand that this entry has such bias.

# Topics on the language itself
ES5 works with most browsers, though there are various execution environments of JavaScript.
If you do your best on ES5 in the first place, you can be freed from confusing compiler (transpiler), that is, [Babel].

As JavaScript has a lot to learn, so first you should think about the option not to tackle with [Babel].

It was impossible for me to choose that option, not facing [Babel]. The reason is simple; I do not want to write a lot of `function`. How about everyone?

I think I should repeat that I came from Java and it is true that I do not have any negative feelings on the process of compiling.

## [Babel] is everyone's sandbox

Designing a programming language is somewhat lonely work. Until now, a few language designers have released the language carefully until they get a shape to some extent.

However, at least in the case of JavaScript, we had learned from ECMAScript 4's failure that it will not work in such a way.

So here comes [Babel], where you can casually implement experimental language features in the form of the plugins of it. Discuss the features of [New language Proposals](https://github.com/tc39/proposals) as using them agressively and decide the features to import into the next version of JavaScript.

Users can just try using cool new features by just writing `.babelrc`. If the feature is not better than expected or is unstable, you can just stop using it.

It is fun to add and subtract language features depending upon your demands.

### [Babel] modules
There are many modules for [Babel], but there are surprisingly few good ones that you should dig into.

The first two are modules for setting up the environment, and the other two are modules for working with other tools.

#### [babel-preset-env](https://github.com/babel/babel-preset-env)
`babel-preset-env` is a handy module that automatically selects the [Babel] plugin to use, in conjunction with the environment where the JavaScript source code runs that is converted by [Babel].

In JavaScript ecosystem, the execution environment of browsers and [Node.js] and so on get improved hand over hand, the plug-in of [Babel] required at compile time changes accordingly.
Though even if unnecessary plugins are activated, what happens is only that ompilation time get slightly increased.

However, it is not very healthy that you can't keep up with the latest state unless you are conscious.

Using `babel-preset-env` will free you from such bold work. Just updating the module on a regular basis will bring you the latest environment, which is just awesome.

#### [babel-register](https://github.com/babel/babel/tree/master/packages/babel-register)
`babel-register` is a hacking module that hooks Node's `require` and inserts [Babel]'s process.

It is not used on loading modules with production code but mainly on running test code.

When executing unit test code in JavaScript, it is common to run a test by detecting a change of file.

If slight modification of the test code kicks compilation of all the project code, you cannot focus on writing code at all.

To avoid that situation, you can fully automate the process of compiling only files that have changed and that are related to those changes by hooking `require` or` import`.

Please note that the test code which does not refer directly to the code under test, such as a test like an E2E test, sometimes does not work very well.

#### [babel-eslint](https://github.com/babel/babel-eslint)
`babel-eslint` is a parser to handle JavaScript code extended by [Babel] with [ESLint].

[ESLint] has evolved properly, so if you write code on ES and JSX as usual, you do not need `babel-eslint`.

The reason why this is necessary is because you use a tool to declare and validate the type in JavaScript called [Flow].

As for [Flow] and [ESLint], I will explain properly later.

#### [babel-plugin-transform-flow-strip-types](https://github.com/babel/babel/tree/master/packages/babel-plugin-transform-flow-strip-types)
`babel-plugin-transform-flow-strip-types` is, as its name says, a module that automatically removes information about types declared for [Flow].

After deleting the information for typing, it converts the written code to JavaScript executable at the target runtime.

On condition with a fixed language specification without using [Flow], [Babel] requires almost no configuration.
So, I think we don't need to repel [Babel] so much.

## Typing system
I'm not going to talk about such a difficult story. It is a story of adding a bit of JavaScript's missing taste.

Since JavaScript has almost no type declaration in the first place, as a simple logic, it is absolutely impossible to know what kind of objects are passed as function's arguments if you just look at the code of the callee side.

If you write both the caller and the callee all by yourself in a short period, you know exactly what to pass as arguments.
Then, you think it's unnecessary to declare them.

But I want you to think calmly. Are you exactly same as yourself a week ago? I can say that it is different. Even though I know roughly what I thought about on writing the code last week, the truth is that I am not sure in details.

It is not a bad idea to provide information to others including yourself in the future by writing comments in the code, but comments are not executed on runtime, and you can not validate automatically.
On the other hand, if you write the type as a little memo, you can automatically verify the code and be happy.

### Faction of type declaration in JavaScript
Let's check out how to declare types in JavaScript here a bit.

#### [Google Closure Compiler]
What I worked on a type declaration for the first time on using JavaScript is [Google Closure Compiler's Type System](https://github.com/google/closure-compiler/wiki/Types-in-the-Closure-Type-System).

If you write a type declaration briefly in the document comment, it validates the types in the code.

```
/**
 * Some class, initialized with an optional value.
 * @param {!Object=} opt_value Some value (optional).
 * @constructor
 */
function MyClass(opt_value) {
  /**
   * Some value.
   * @private {!Object|undefined}
   */
  this.myValue_ = opt_value;
}
```

What makes this approach wonderful is that the type declaration is in the comment, so the programmer can provide the necessary information by the compiler without affecting the behavior of the source code at all.

On the other hand, there is a problem that the declaration in the comment is easy to be ignored. There is also a problem that expression flexibility is extremely low instead of being easy to understand.

`ADVANCED_OPTIMIZATIONS` of [Google Closure Compiler] has a risk that the code will not run at all, but I was really surprised because the code size gets seriously smaller.

#### [TypeScript]
[TypeScript] emerged as one of a lot of efforts to develop a new language as an advanced JavaScript.

For me, I think that [TypeScript] is a kind of grace from God, with the recognition that it is the latest work by Anders Hejlsberg (https://github.com/ahejlsberg) who designed Delphi and C#.

Let's look at the code of [TypeScript] a bit.

```
interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person: Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

var user = { firstName: "Jane", lastName: "User" };

document.body.innerHTML = greeter(user);
```
[TypeScript] is more similar to the my own Java I mentioned earlier than the originial Java.

[TypeScript] provides 2 ways to daclare types; one is to mix type annotations in the code to work and the other is to create a dedicated file for type declaration.
For example, if you create a dedicated file, declare it like this.

```
declare namespace myLib {
    function makeGreeting(s: string): string;
    let numberOfGreetings: number;
}
```

If you have a file with the extension `d.ts` for type declaration, you can write the code as if there is a type in a library, even if that has no actual type declaration.

It is a crucial feature to be able to have types in post-installation of the libraries with out tyep declaration as needed in order to utilize existing assets.

The [DefinitelyTyped] project that was working to collect type declaration files for OSS modules into a single repository was busted due to the enormous number of declaration files.

Now the organization called [types](https://github.com/types) is managing them in a straightforward way where allocating one repository for one module.
Nonetheless, it is not to say [DefinitelyTyped] is not referenced at all.

By the way, to retrieve the type declaration files from the npm repository, you can do `npm install -D @types/lodash` etc. It's great that you can get them by npm, not by the decicated tools.

#### [Flow]
[Flow] is a static analysis tool written in OCaml, and it checks the type declaration mixed in JavaScript, and it performs various validations.

Let's see a bit of JavaScript code typed in [Flow].

```
// @flow
function total(numbers: Array<number>) {
  var result = 0;
  for (var i = 0; i < numbers.length; i++) {
    result += numbers[i];
  }
  return result;
}

total([1, 2, 3, 4]);
```

It looks like it is similar to typeScript's type annotation. But [Flow] has a distinctly different point from [TypeScript].
[Flow] does static analysis such as evaluating type annotation given to JavaScript, but does not create a new programming language.

It is a closer approach to [Google Closure Compiler]. However, unlike the time when [Google Closure Compiler] was created, now there is [Babel], so once you have extended the syntax of the language primarily and you finish using it, it is possible to safely remove only the description specifically for [Flow] implementation.

Type declarations done in comments is limited in terms of expressive power. [Flow] got over the challenge. Nevertheless, compared with Haskell and OCaml, it does not have much rich expressiveness, even compared to Java.

It is not so difficult to understand the extent where [Flow] infer the type. In other words, [Flow] does only very simple type inference.

For [Flow], there is a repository called [flow-typed] for sharing type declarations like [TypeScript].
I do not know why it's going to do the same mistake as [Definitely Typed], but it's managing them in a single repository.

However, this seems to reviews the PRs serverly, and the contents of the stored type definition file are strict and the absolute amount of those are small.

On retrieving a type declaration file, the command `flow-typed` will automatically download the type declaration files of dependent module in package.json from the [flow-typed] repository.
For those without a type declaration, it makes a type declaration file where all types are declared as `any`.

Why does not it let you pull type declaration files with the `npm` command? I wonder it should have another command that make a miscellaneous type declaration file.

### Which type system to use
There is no reason to choose [Google Closure Compiler] anymore.

Since [TypeScript] and [Flow] are different objectives, they are not something that can simply be evaluated in the score sheet. If you compare it in the Star table, [TypeScript] is a victory.

Well then, which one to use?

It'd be good to consider the support for the libraries and the frameworks you want to use.

At least if you use [Angular], [TypeScript] is the choice and if you use [React], definitely you use [Flow].

[There is an official TypeScript type definition file for Vue.js](https://vuejs.org/v2/guide/typescript.html). However, this does not recommend the use of [TypeScript], it seems that the contribution by some users was merged.

If you use normal JavaScript, it is good to use [Flow], and [TypeScript] is a pretty good choice if you have the type definition files of the library you'd like to use.
Especially [VS Code] apparently improves the precision of input completion if you have the type definition files.

Supplementally, [Flow Language Support](https://marketplace.visualstudio.com/items?itemName=flowtype.flow-for-vscode) also has a function to enhance input completion.

## Unresolved issues in the type system
Supplement the indications received from experts after the first release of this entry.

* [Dart](https://www.dartlang.org/)
  * A language that makes stronger type inference than [TypeScript]
  * [Dart2js](https://webdev.dartlang.org/tools/dart2js) can convert the code to JavaScript
  * Two large use case [Angular2] within Google's [already exists](http://news.dartlang.org/2016/03/the-new-adwords-ui-uses-dart-we-asked.html)
  * Large use case is achieved with [Angular 2 Dart](https://github.com/dart-lang/angular2)
 
## Let's share best practices with static analysis
JavaScript has many traps. The trap here is the language specification that many programmers tend to misunderstand, and the behavior of certain libraries.
Because JavaScript is a truly flexible language, most of the new language specifications can be converted directly into the old language. That is why there are many traps.

Code written as results of mistakes by programmers, code not written unless misunderstanding about specifications, code that can be considered a bug in most situations, and code which satisfies the language specification but can not obtain the expected results on runtime in an application... Lint is a tool for automatically finding those codes.

If you use Lint a bit more aggressively, you can inspect anything that has nothing to do with the behavior of the code but with code readability, such as how to indent and how to name variables.

### History of Lint in JavaScript
If you take a glance at Javascript ecosystem, you will find lots of Lint tools. The oldest one that I have used is [JSLint](http://www.jslint.com/).
This guy isthe Lint made by legendary [Douglas Crockford](https://github.com/douglascrockford) and is very strict. It's an hardcore style lint tool that does not allow any indulgence.
It is painful for unskilled guys because it makes us feel the author's strong will that codes that can do strange behavior should be a strange look and he would never misled by a strange behavior of JavaScript.

By the way, [reading the code of JSLint](https://github.com/douglascrockford/JSLint) is a great resource to learn JavaScript. It's compact and easy to read.

Lint that became evangelical for unskilled guys like me is [JSHint](http://jshint.com/). This is not too strict. It is incredibly easy to use because it can cut out the setting file.
However, JSHint is easy to enable and disable the built-in functions, but adding a new rule is a little troublesome.

Also, to incorporate someone's recommended rules into your project, you have to copy and paste the settings.
Most people want to use someone's recommendation, and only a part of maniacs like me like to make Lint's rules.
If that recommended set is maintained, you want to use only that final result without doing troublesome things.

That's why [ESLint] is now recommended that is easy to extend and easy to create configuration files.

[ESLint] has the excellent plug-in system, so there are plenty of plug-ins, and you can publish the suggested rule set as an npm module.

Airbnb's published [eslint-config-airbnb](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb) is one such kind of recommended rule set.

Airbnb's one contains a lot of things that do not fit my idea, so I did not adopt it, tho.

### [ESlint] related modules
[ESLint] related modules are almost all about plug-ins.

I listed here those at least that I used and found especially useful though I think there are many other modules.

If you are an [ESLint] mania and know a useful plugin I do not know, please let me know.

#### [eslint-plugin-ava](https://github.com/avajs/eslint-plugin-ava)
In my project I use a testing framework called [AVA]. [AVA] will be explained later.

[AVA] is relatively straightforward and easy to use, but it is too simple to understand how to write the test code until you get used to it.

So, using this plugin makes it possible to get warned when you use [AVA] obviously incorrectly.

#### [eslint-plugin-import](https://github.com/benmosher/eslint-plugin-import)
`eslint-plugin-import` finds errors on` import` statements.

In concrete, if you try to `import` a module that does not exist in the project, you will get a warning. Everyone can typo the module name.

In another example, if you pass strings other than literals to `require`, you get errors.

#### [eslint-import-resolver-node](https://github.com/benmosher/eslint-plugin-import/tree/master/resolvers/node)
Only this one has a different role from other modules.

`eslint-import-resolver-node` can work with `eslint-plugin-import` to customize how to find `import` modules.

What I mean is, in my project, I store application code and test code in separate directories, `src` and `test` respectively.

In this case, in the test code side, when you do `import` application code, I have to write ` import world from '../../src/hello/world";`, and it is painful.
To avoid that, by setting `NODE_PATH=src` when executing the test code, we do not need to write that `../../src/`part.

I use `eslint-import-resolver-node` to inform the omission to `eslint-plugin-import` about this.

#### [eslint-plugin-promise](https://github.com/xjamundx/eslint-plugin-promise)
The JavaScript `Promise` is a type of library you need to get used to. Moreover, it is hard to understand the correct usage.
Speaking of `Promise`, [JavaScript Promise's book](http://azu.github.io/promises-book/) is a very wonderful document so I recoomend to read it many times.

It is highly likely that you are using `Promise` wrongly until you get used to `Promise`.
If there are JavaScript experts around you and can receive code reviews from them, it is very wonderful. But I am writing a bit of JavaScript as a hobby, I can not ask such an expert casually.

So by using this plug-in, you will get errors in the obviously wrong `Promise` code.
You can understand how to use `Promise` by simply reading the rule details of this plugin, so please try setting the rules carefully.

By the way, if `async/await` are introduced into ES, will not you use `Promise` directly?

I think that `async/await` and `Promise` can coexist according to my experience of using C#'s `async/await`, but I don't exactly know how they are in JavsScript.
At least, in my impression using `async/await` in [AVA] has effect to make test code easy to understand.

#### [eslint-plugin-security](https://github.com/nodesecurity/eslint-plugin-security)
Do you care about security while writing code?
I am doing security reviews using static analysis tools as a job, so I'm paying attention on it to some extent.
However, I am not whenever there is a consideration on security in the head.

The amount of vulnerability that can be checked with this plug-in is not much, but it finds vulnerability that easily get mixed in.

As long as you write the code normally, you may not see any errors due to this plugin, but when you get warned by this plug-in, I want you to be cautious a little.

#### [eslint-plugin-flowtype](https://github.com/gajus/eslint-plugin-flowtype)
Although it is good to type declaration using [Flow], sometimes you may make a huge mistake.

Also, since the type declaration by [Flow] is not ordinary JavaScript, the standard rules in [ESLint] are not applied at all.
In other words, you need to reimplement rules for [Flow], such as indentation, sticking a semicolon at the end of the line, killing trailing comma, etc.

[Flow] itself has its traps, and if you declare a consistent type declaration, it would be better to keep them linted.

I told the same story at `Promise`, but it is good to use `eslint-plugin-flowtype` as a training plaster jacket since [Flow] is a new technology.
If you get used to it properly, no error will come out at all.

#### [eslint-plugin-react](https://github.com/yannickcr/eslint-plugin-react)
In my project [React] is used as a framework for building UI.

Since [React] is very commonly used, along with many best practices, undesirable coding styles have been found.

Everything defined as a rule is not always desirable for our project, but it is much easier to argue about what is being ruleized than to create that knowledge.

In fact, to get knowledge about the rules as defined in this plug-in by yourself, it will not be enough to simply use it with a small project.

The merit of using a de-facto standard framework lies in this place.

#### [eslint-plugin-jsx-a11y](https://github.com/evcohen/eslint-plugin-jsx-a11y)
How carefully are you using the `Accessibility` of the system that we are making?

I feel that it is `Accessibility` that has a lower priority than `Security` which tends to be negligible.

Because it is to provide a rich UI for a better user experience, it may be good to give consideration to users with disabilities on the extension line.

However reading through [WAI-ARIA 1.1](https://www.w3.org/TR/2016/CR-wai-aria-1.1-20161027/) and [MDN's ARIA page](https: / /developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA) from end to end is extremely hard if you are to do it.

I hope to provide the maximum value as much as I can with the munimum effort, such as referencing to [HTML5 Accessibility](http://www.html5accessibility.com/).

So I recommend to make sure first that the JSX in the project uses the `eslint-plugin-jsx-a11y` recommended rule to deal with the `Accessibility` support.

# Testing
There are variety of tests. Here we talk about developer testing.

I think the technology that the programmer should master the most in application creation is tests.
It is because I believe that better ones can be made by expressing what it is supposed to be in the form of tests and by implementing based on those.
In other words, I do not want to talk that I should be able to do something that a real test engineer is doing.

For quality assurance, there is enormous systematized knowledge, and it would be great if developers can aquire the knowledge both to make applications and to assure the quality, but the time is finite.
They have lovely families or fancy games to play with at night. I want to sleep ten hours a day to live long; I want to eat delicious meals slowly. Again, time is finite.

You should spare time learning what to test, so do not take time to learn how to use the testing framework coolly.

That's why the testing framework we should chooses is the one with APIs that is as simple as possible.

That is, it is [AVA].

## Recommendation [AVA]

The test code written in [AVA] is like this.

```
import test from 'ava';

test('foo', t => {
    t.pass();
});

test('bar', async t => {
    const bar = Promise.resolve('bar');

    t.is(await bar, 'bar');
});
```

Let me briefly list why I like [AVA].

* API is small
* [Power-assert] is built-in as standard
* Faster execution speed
* Perfect support for the `=>` operator, `Promise`,`async/await`, [observable](https://github.com/tc39/proposal-observable)

### [AVA] API is small
After importing with `import test from 'ava';` you can do whatever you want by calling the function defined in the `test` variable.

And there are only 11 functions including this `test` default function. Let's enumerate those.

* **test([title], implementation)**
* test.serial([title], implementation)
* test.cb([title], implementation)
* test.only([title], implementation)
* test.skip([title], implementation)
* **test.todo(title)**
* test.failing([title], implementation)
* test.before([title], implementation)
* test.after([title], implementation)
* **test.beforeEach([title], implementation)**
* **test.afterEach([title], implementation)**

The only thing to keep in mind is the four highlighted APIs. Other than that, you can refer to the manual when they get necessary.

It is also important that [AVA] does not work on global space or functions placed in implicit `this`.

### [power-assert] is included as standard
When writing tests, I recommend to use [power-assert] which will give you the richest assertion error if you use it without thinking anything.

This test generates:
```
test(t => {
    const a = /foo/;
    const b = 'bar';
    const c = 'baz';
    t.true(a.test(b) || b === c);
});
```

This kind of error:
```
t.true(a.test(b) || b === c)
       |      |     |     |
       |      "bar" "bar" "baz"
       false
```
This is just awesome!

[power-assert] rewrites the code to issue this assertion error, but the setup for that is a little troublesome. It is easy to do, and manuals for it are fully available, but well, it is bothersome still.

But, if you use [AVA], it does not require such a little troublesome.

### Faster execution speed
When the speed at which the test is executed becomes a problem is after releasing a real application and entering the maintenance phase.

While running for a while, it doesn't matter if the execution speed is 5 seconds or 1 second. But what if the amount of test case is 1,000 or 10,000?

It is no longer possible to run all the tests on your local machine each time you change the code.

Just run the test code that seems to be directly related to the part you changed, and then just throw it to a CI server like Jenkins or CircleCI, and pray with hope that somewhere I do not expect doesn't fail... However, as you expected, it fails.

Since the test always fails regularly, it is better to shorten the cycle of change → test → debug → change ......

[AVA] is designed not to dare to write a test code of a very complicated structure to secure the test execution speed.
For a person who wrote a complicated structure test with a testing framework like [Mocha](https://mochajs.org/), it may seem strange, but it is a matter of habit.

Accumulated variable scopes of test code is good when writing, but after all, it will be a pain.

### Perfect support
It is just said that the API of [AVA] can be perfect as it is simple.

Since `this` is not rewritten badly, the `=>` operator works comfortably.

Since the return value of the test method is determined that it is within the responsibility of [AVA], even if it is `Promise` or [observable](https://github.com/tc39/proposal-observable) or any others, [AVA] will do it for you.

Function calls listed in test code files won't be nested.

By the way, the following is how to reference the variable created by `test.beforeEach` from `test`:

```
test.beforeEach(t => {
    t.context = 'unicorn';
});

test(t => {
    t.is(t.context, 'unicorn');
});
```

`context` is not shared among multiple `test`, so you can touch it as much as you want.
However, the execusion order of multiple `test` are not guaranteed unless `test.serial` is used, so do not modify `context` in `test`.
In the first place `test.serial` should not be used unless there are any special reasons.

Do not share the infomation among test methods and write test in a way that the test execution order has meaning, because it is a bad practice.

## Unresolved issues in testing
The followings are what I should learn, but I hope someone will supplement the tasks that I haven't researched well yet.

* E2E test or system test
  * It seems that [React application can be tested](http://www.joelotter.com/2015/04/18/protractor-reactjs.html) using [Protractor](http://www.protractortest.org/) 
* Mutation test
  * [Stryker](http://stryker-mutator.github.io/) looks okay but there is no [AVA] support.
* Stress test
  * In particular, there should be a rich UI load test methodology and a tool made in JS for it, but I couldn't have found one yet.

# How to make a UI
From here, I will explain how to construct UI with JavaScript.

# History of UI libraries

When I used Gmail and Google Maps for the first time about ten years ago, I was surprised how such high-performance and rich screen expression can be done with JavaScript alone.

Though there was a group of people who tried to make dynamic websites by DHTML where JavaScript is used to move things using JavaScript, they had been abhorred in general.

To run an application with a rich user interface in the browser, we had to use Flash. There were other technologies like browser plug-ins, but in the end, they were rarely used.

Activities like implementing GUI applications that work in realistic performance using only JavaScript on the browser have been active since then, but until the last few years, those efforts had not worked not that well.

Although libraries such as [YUI Library](http://yuilibrary.com/), [Ext.js](https://www.sencha.com/products/extjs/), [Closure Library](https://developers.google.com/closure/library/) came up one after another, it had been incredibly difficult to make a UI like what was made with VB and Delphi.

For example, it was not realistic to display grid control such that each cell performs a complicated operation with displaying about 1000 data.

As a result, in addition to CSS, it became a standard to work hard a bit with jQuery. If you only want to make a website with a little dynamics, you can do it with jQuery.
If you do not work with a large number of people, do not maintain for a long time, or do not make extraordinary things, it should work fine.
Most websites are not GUI applications, so you can quickly create a website with jQuery and its plugins conveniently.

By the way, is there a clear boundary between highly-built websites and GUI applications running on browsers?

I have never seen quantitative judgment criteria for it, but at least the one with dynamic behaviour would be a website that a few web designer can create without hurting by using jQuery.

On the other hand, the one should be a GUI application on the browser that has server side application, that requires three or four programmers to create the UI, and that can't be achieved without design boundary between UI design and its logics.

Actually it is not such an easy task to distinguish these so easily.

The reason why this distinction is important is that the toolkits to use and skill sets of team members to be aligned are different.

If you try to create a website using a framework for implementing sophisticated GUI applications like [React] and [Angular], you will waste a lot of human resources.

On the other hand, if you are planning to implement sophisticated GUI applications using jQuery plugin, you will doom.

Tools should be properly used in appropriate places.

## Breakthrough called Virtual DOM
The task to be surely solved in making GUI applications that run on browsers is performance and usability.

Especially the problem of performance is tough to solve. Since it operates on the browser, all UI elements are dropped into the DOM in some form.

First, since DOM has a general-purpose tree structure, memory efficiency is terribly bad.
Also, this tree structure can not limit the number of intermediate internal nodes, and the depth of the terminal node can not be limited, so the cost of scanning tends to be high.

For the browser to draw the DOM on the screen, it is necessary to scan all nodes in some form, so this poor efficiency becomes a big problem.

Also, the mechanism of laying out drawing elements completely ignoring the DOM structure called CSS spurs poor performance.

It is natural in the browser world, such as having to redraw the entire screen with one class attribute that appeared at the terminal node.
In other words, it is necessary to lock the entire screen just by `appendChild` the newly created node for the DOM already drawn.

However, if the impact of the new node on the entire screen is small enough, the time to keep securing locks will be sufficiently short.

Just updating certain components on the screen may cause redrawing of the entire screen, so you need to pay close attention not to do so.
In such a case, except for some exceptional organizations, it is not possible to efficiently create large-scale GUI applications.

Virtual DOM is the technology that answers this problem.

First, consider the data (model) necessary for rendering the screen and the template (view) into which the data is poured.
If you add a controller that has the role of updating the model from user input to this, it becomes a classic MVC model.

In a primitive way, programmers decided which part of the view to change, depending on which of the components of the model was changed. Here is a very simple example.

Consider putting the variable `user` in the first half into the HTML-like template in the second half.

```
// Example model
var user = {
  "name": "John Doe",
  "age" : 22
}
// View example
<html><body>
<div class="name">
Name: ${user.name}
</div>
<div class="age">
Age: ${user.age}
</div>
</body></html>
```

Updating the model means changing the contents of `name` and` age` which are members of the variable `user`.
In a primitive way, the programmer must write code to find the corresponding DOM element, depending on which member variable you changed.
Indeed, in this simple example, you can find the relevant element by writing `$(".name")` in jQuery.

Think about it, if there are 1,000 pieces of changeable elements on the screen?
What if some elements change, and there is an element that needs to be transiently changed depending on it?
What if my component should be changed, affected by elements that did not exist when I incorporated the component into the screen?

It's a nightmare.

In summary, Virtual DOM takes the difference between the current DOM and the next DOM output and passes only the difference to the browser's rendering engine.
It is a mechanism that automatically does the code which was written by the human beings in partial spirit up to date.
Of course, it may be slower than an implementation where craftsmen choose elements to be updated one by one.

In reality, there are only a few rare artists, so those who automatically get the differences will operate at high speed.

A similar argument was from around the time of C language and assembler.
If you hand assembled all the applications, you can make binary faster than binaries issued by ordinary compilers, that's the story.
It is a story that can not be established unless the object to be hand assembled is limited to a small size or the application is not small enough.

## Which framework to use

Roughly speaking, what I made the candidates for selection is this. In any case, they use Virtual DOM internally.

* [React]
* [Angular]
* [Vue.js]
* [Mithril]

### [React]
Current status [React] can be said to be the de facto standard, so it would be most reasonable to select [React] and learn.
The community is sufficiently large and peripheral tools are also substantial.
There are also many good quality tutorials like [SurviveJS](https://survivejs.com/).

That's why I selected [React] in my project.

There is also a framework compatible with [React] such as [Inferno](https://github.com/infernojs/inferno) and [Rax](https://github.com/alibaba/rax)
If there is plausibility in what they are doing, it will be incorporated into [React] either.

### [Angular]
[TypeScript] is designated as the primary language [Angular] is a fascinating choice for me.
However, as far as I can observe, the community is not sufficiently large compared to [React].
This is due to the relatively short period of stable releases of stable ones.

The current [Angular] is 2.4, but the first type of [Angular] is my favorite type of framework.
Anyway, there is a 2way binding; there is a Directive where every black magic act is allowed as a garbage repository, there was a DI container.
The digest loop was a very difficult concept, but I thought that complexity should be accepted to make the screen easier to make.

The first line of [Angular] was a framework that seems to exist for SIer in a function group that seems only to be designed for creating business applications.

I think that it is not too late to drop a large learning cost on [Angular] 2 system even after full-scale adoption case inside Google is released.

### [Vue.js]
[Vue.js] is a topical framework right now. I liked the [Angular]1 The one I should choose is [Vue.js].

But, I can not adopt what is [the main developer is only one](https://github.com/vuejs/vue/graphs/contributors) to the base part.
[Evan You](https://github.com/yyx990803) It can be a catastrophic situation if one person loses a little enthusiasm, or just transforming by some political transaction.
It's a terrible thing, and it's an unacceptable risk to me.

Randomly as far as I can read the code base gets a reasonable impression as such and there is quite polite documentation.
It seems there is no shortage in function with the feeling that I tried a little.
However, when I thought about making a large application as appropriate, I pulled the fundamental problem in an unexpected place and thought about the possibility that I could not cope well with that.

There are large communities, and those that are used in a live application have been discovered and carefully dealt with many fine problems.
The code which seems to have meaningless complexity at first glance may also have important meanings in certain circumstances.

This is only my bad impression theory, but the second line of [Angular] and [Vue.js] have the beauty of the invisible code base as used in a serious production environment.

By the way, the first line of [Angular] was a code base that seems to , and some in production environment.

### [Mithril]
The goodness of [Mithril] is small. After fully understanding the internal details of the framework, it is a tool used by people who can add it myself by myself by noticing what is missing.

Because it is small, you can do it.

It may be suitable for use with a small team composed only of engineers with sufficient development experience of GUI applications.

It is really difficult to properly determine the specifications of those that are missing and properly implement it, assuming that they are missing while making the application.
Especially when the cost pressure during project progression is high, can it do well? At least, I will not be able to do it.

### About [React]
Since the body of [React] has a role only for the part of the view, there are various shortcomings in making the application.

I chose [react-router](https://github.com/ReactTraining/react-router) without much trouble about Router. It can be said that I do not have standards for selecting Routers.

As a utility for testing, you can not remove [enzyme](https://github.com/airbnb/enzyme).
It is the greatest merit of using a de facto standard library that you can use high-quality modules created by users who have incorporated frameworks.

### About [CSS Modules]
In other words, CSS is like a programming language with only global variables, a programmer who lives in a world where the default scope is local variable is an unavoidable environment.
In an environment where the complexity rises in proportion to the amount of code, I think that it is impossible to make perfectly consistent work.

What we have been dealing with this problem up to now has been to divide the namespace by explicitly defining the naming conventions by humans.
For example, [OOCSS](http://oocss.org/), [BEM](http://getbem.com/), and [SMACSS](https://smacss.com/) are included in major naming conventions is there.

Also, write CSS more securely using meta-language such as [Sass (SCSS)](http://sass-lang.com/) or [less](http://lesscss.org/) Efforts have also been made.
The most important feature of these meta-languages ​​is to limit the scope of influence by nesting the definitions of styles.

The meta-language is excellent as a mechanism to control the complexity of the CSS itself, but since the CSS is a mechanism for deciding what kind of appearance is given to the DOM structure in the first place, the design of the DOM structure and the design of the CSS can not be separated.

If so, it is desirable that the design of the [React] component and the design of the CSS are consistent. It would be impossible to treat each as a completely independent event.

[React] to ensure consistency between the design of the component and the design of the CSS, either way, is the priority. In that case, it is desirable to bring local variables to CSS and make it the default behavior.

The concepts and efforts devised to realize this are [CSS Modules].

[CSS Modules] is a concept derived from the community of [React], but similar functions are also found in [Vue.js](https://vue-loader.vuejs.org/en/features/css-modules.html),
[There is a similar function in Angular 2](http://joaogarin.github.io/css-modules-angular2/) Looks like.

Detailed information on the CSS problem resolved by [CSS Modules] is [There is a blog entry by members of the CSS Modules team](http://glenmaddern.com/articles/css-modules), so please refer to that.

To summarize, [CSS Modules] is not just a kind of CSS design methodology based on new naming conventions. Also, using [CSS Modules] does not mean you do not have to use the CSS meta language.
Even web designers were accustomed to existing naming conventions should learn [CSS Modules] if you are doing UI design with projects that share work with to add.

Even if you use [CSS Modules], CSS designed to make UI consistent for the entire application will be defined in the global space as before, so existing CSS It does not mean that you do not need design knowledge on the design.

### [React] in [CSS Modules]
To introduce [CSS Modules] to [React], use [css-loader] of [webpack] to process CSS and then write a dedicated description to the [React] component side. [Webpack] will be explained later.

#### Introduction of [CSS Modules]
The easiest way to use [CSS Modules] is this code.

```
import React from 'react';
import styles from './table.css';

export default class Table extends React.Component {
    render () {
        return <div className={styles.table}>
            <div className={styles.row}>
                <div className={styles.cell}>A0</div>
                <div className={styles.cell}>B0</div>
            </div>
        </div>;
    }
}
```

When this is rendered, it will be roughly like this HTML. Because CSS class name which does not automatically duplicate by [css-loader] is named, the weird name is set in the class attribute.

```
<div class="table__table___32osj">
    <div class="table__row___2w27N">
        <div class="table__cell___1oVw5">A0</div>
        <div class="table__cell___1oVw5">B0</div>
    </div>
</div>
```

The problem is that importing CSS as if it is a JavaScript object is handled this way.

In this case, the CSS must also be applied to the component during unit tests that do not need to evaluate the state of the style at all.
If an import is processed in a naive manner as it is, it becomes an error because it can not parse import destination CSS as JavaScript.

It is hard to write code with many curly braces as a code, so I want to avoid it if possible.
Braces are difficult to type because they require Shift.

#### Improvement with [react-css-modules](https://github.com/gajus/react-css-modules)
There is [react-css-modules] as a module to remedy the problem that occurs when using [CSS Modules] in the simplest way.

Using this makes it such a code.

```
import React from 'react';
import CSSModules from 'react-css-modules';
import styles from './table.css';

class Table extends React.Component {
    render () {
        return <div styleName='table'>
            <div styleName='row'>
                <div styleName='cell'>A0</div>
                <div styleName='cell'>B0</div>
            </div>
        </div>;
    }
}

export default CSSModules(Table, styles);
```

Instead of the braces disappearing, we now import [react-css-modules].

A special property called `styleName` has been introduced which is added to the class attribute when rendered.

Although code easiness was improved, the difficulty of the code has not improved much.

Especially, as you can see from the last line, the problem of handling imported CSS as a JavaScript object has not been solved.

#### Improvement with [babel-plugin-react-css-modules]
There is [babel-plugin-react-css-modules] as a plugin of [Babel] to solve the problem of [react-css-modules].

By the way, the developers of [react-css-modules] and [babel-plugin-react-css-modules] are the same.

Using this makes it such a code.

```
import React from 'react';
import './table.css';

export default class Table extends React.Component {
  render () {
    return <div styleName='table'>
      <div styleName='row'>
        <div styleName='cell'>A0</div>
        <div styleName='cell'>B0</div>
      </div>
    </div>;
  }
}
```

You can now avoid handling CSS as a JavaScript object. We use import statements only to show the relationship between CSS and components.

[babel-plugin-react-css-modules] performs processing necessary as [CSS Modules] at compile time.
Thanks to that, the component code does not increase only for [CSS Modules].

It is nice to have the code refreshed in this way.

Since import of CSS does not affect components on code, removing such a part with a module like [ignore-styles](https://github.com/bkonkle/ignore-styles) has no adverse effect.

So in my project I decided to use a combination of [css-loader] and [babel-plugin-react-css-modules] to implement [CSS Modules].

### About [Flux] implementation
It is good to adopt [Flux] as the architecture that defines the structure of the application.
There are many libraries implementing [Flux], but since it is popular and easy to use, [Redux] is used.

Regarding using [Redux], how much roles and responsibilities are given to Actions, and ActionCreators depends on the size of the application, the optimal solution changes.

Using [reducex-thunk](https://github.com/gaearon/redux-thunk) or [redux-promise](https://github.com/acdlite/redux-promise) makes it very Although easy to understand, the ActionCreator code tends to become large.
As the code related to asynchronous processing appears in Action, measures must be taken to set some criteria and separate codes from Action in the process of increasing the code base.

In a very simplistic way, `redux-thunk` implements Action in the callback model. `Redux-promise` implements Action in the` Promise` model.

The problem with this simple approach is that you can not cancel the task.
It is common in GUI applications that there is some inconsistency between the server and the client, and users who are irritated by the response delay want to interrupt processing.
In such a case, it is not desirable that there is no way to cancel the task being processed with the user operation as the starting point.

[redux-saga] and [redux-observable] are modules that are quite difficult to understand the behavior model.
Instead, the ActionCreator code will be simpler, and Action will be a fairly simple object to store the contents of the event that occurred.

[reducex-saga] and [reducex - observable] add a new layer whose name is different but has the same role to [Redux].

Again, for simplicity, [redux-saga] implements additional layers using GeneratorFunction and [redux-observable] uses [RxJS] to implement additional layers.

Both [redux-saga] and [redux-observable] have a way to cancel the task.

When writing code to handle errors, the difference between [redux-saga] and [redux-observable] becomes apparent.
For details, see the individual documents, but the point is whether to use `try/catch` or use the event definition handler of library definition.

* Redux-saga's [Error handling](https://redux-saga.github.io/redux-saga/docs/basics/ErrorHandling.html)
* RxJS [Error Handling](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/gettingstarted/errors.md)

How I tried to say, I dislike the `try/catch` syntax which is only a legal `goto`. Although I write it without any help, I do not want to write positively.

Here is a summary of the discussion so far.

<table> <thead>
<tr>
<th align = "left"> Name </th>
<th align = "left"> Implementation Technique </th>
<th align = "center"> difficulty </th>
<th align = "center"> Action </th>
<th align = "center"> Additional layer </th>
<th align = "center"> Cancel </th>
<th align = "left"> Error Handling </th>
</tr>
</thead> <tbody>
<tr>
<td align = "left"> redux - thunk </td>
<td align = "left"> callback </td>
<td align = "center"> low </td>
<td align = "center"> Large </td>
<td align = "center"> unnecessary </td>
<td align = "center"> x </td>
<td align = "left"> try / catch </td>
</tr>
<tr>
<td align = "left"> reducex-promise </td>
<td align = "left"> Promise </td>
<td align = "center"> low </td>
<td align = "center"> Large </td>
<td align = "center"> unnecessary </td>
<td align = "center"> x </td>
<td align = "left"> event handler </td>
</tr>
<tr>
<td align = "left"> reducex - saga </td>
<td align = "left"> Generator </td>
<td align = "center"> Medium </td>
<td align = "center"> Small </td>
<td align = "center"> required </td>
<td align = "center"> ○ </td>
<td align = "left"> try / catch </td>
</tr>
<tr>
<td align = "left"> redux - observable </td>
<td align = "left"> Rx </td>
<td align = "center"> high </td>
<td align = "center"> Small </td>
<td align = "center"> required </td>
<td align = "center"> Yes </td>
<td align = "left"> event handler </td>
</tr>
</tbody> </table>

That's why I chose [reducex-observable] in my project.

## Unresolved issues in UI
I should learn, but please write a little, hoping that someone will supplement the tasks that have not been done yet.

* Talk about usability in JavaScript GUI application
  * Talk of methodology to respond to keyboard-centered operations, operations using pointing devices other than mouse, operation by touch display, etc.
  * Talk of evaluation criteria of operability in web application
* [PostCSS]
  * In my project, I use it as [babel-plugin-react-css-modules] asks for it
  * In something like [Babel] in the CSS area, you can plug in and uninstall features by plugin ... only understand to the extent
* [CSS Flexible Box Layout Module Level 1](https://www.w3.org/TR/css-flexbox-1/)
  * As far as we can see, the Flexible Box is the layout engine of the GUI application itself
  * The material of [CSS Flexible Box Layout](https://developer.mozilla.org/en/docs/Web/CSS/CSS_Flexible_Box_Layout) in MDN is straightforward
  * Since it is under development of specifications, it works only with the latest browser
* [CSS Grid Layout Module Level 1](https://www.w3.org/TR/css-grid-1/)
  * As far as we can see, Grid Layout closely resembles Java's GridBagLayout.
  * Since the specification is under development [It does not work on browsers released as stable version](http://caniuse.com/#feat=css-grid)

# Topics on Build
I talked about basic language talking, testing method, how to make UI, next is build.

Continuous integration (CI) is essential in a contemporary development process.
In other words, you have to automate the build process with CLI. It will not change even if you develop it in the Windows environment.

SaaS of CI that I use is [CircleCI] and [AppVeyor].

[CircleCI] is used to build in Linux environment. Especially when the build fails, you can debug and collect logs by [SSH login](https://circleci.com/docs/ssh-build/).

It often happens that the build results on the CI server where the environment gets beautiful every time the environment does not match the build results of the local environment where various things are stacked.

Especially, in my case I am developing in Windows environment, so it is easy for the environment to differ from CircleCI in Linux environment. That is, SSH login is a highly important feature.

[AppVeyor] is used to build in a clean Windows environment. Just a little crafted the configuration file; you can debug and collect logs by [can connect to RDP](https://www.appveyor.com/docs/how-to/rdp-to-build-worker/).

The OS is Windows Server 2012 R2 (x64), but it does not use it so that the difference between the server OS and the client OS becomes a problem.

About managing dependency modules
Based on package.json which stores meta-information about the project in Node, it comes with a tool called npm that can execute various tasks.

Especially important among tasks to be executed from npm is automatic downloading of dependent libraries.

Although npm is done very well, `npm-shrinkwrap` for fixing the dependency library version is very hard to use.
If you do not explicitly execute the `npm shrinkwrap` command in the first place, the file that preserves the state of the dependency library is not created.
Furthermore, there is no indication that the `production` mode [bug that effectively does not work](https://github.com/npm/npm/issues/11189) will be fixed.

So I decided to use [Yarn] as an alternative to npm in my project.
[Yarn] creates a file that saves the state of dependent libraries unless explicitly specified with command line options.
Of course, the `production` mode works properly.

After fixing the dependency library version with [Yarn], please use my [ci-yarn-upgrade](https://github.com/taichi/ci-yarn-upgrade).
It periodically monitors the state of dependent libraries and automatically generates a pull request to update package.json and yarn.lock if necessary.

About the task runner
Writing a build script that runs on multiple platforms imposes a limitation that shell scripts can not be used.

If I have a Linux binary [PowerShell](https://github.com/PowerShell/PowerShell) you can do it, right? It is because it is a Windows user.

Since the language used in the project is JavaScript and Node works properly on multiple platforms, it is preferable to write the build script in JavaScript.

There are many modules for writing build scripts with JavaScript, but my way of thinking is simple.

Do you use [gulp] or not?

Indeed, there are task runners like [Grunt](http://gruntjs.com/) and [broccoli](https://github.com/broccolijs/broccoli).

The reason why these can not be taken into consideration is simple because it does not provide a convenience that greatly exceeds npm script.
Since there are many objects to be learned at all, I do not want to pay learning costs to the task runner as much as possible.

Still, the reason why [gulp] can be considered is performance. [gulp] is a task runner that places Node 's Stream API at the center of task construction and operates at high speed.

In the code base of hundreds of thousands of rows level, it is expected that the execution time of the build can be shortened as much as possible, as well as the test execution time should be shorter.

By the way, there is a thing called "do not predict" to what I like in the maxim of performance.
In other words, if the reason for adopting [gulp] is only performance, its adoption is that the code base grows big enough that even if the build time is long enough it, will not be late.

So, in my project, I decided to try as hard as possible with npm script.

### Useful module for npm script
There are several useful modules for writing build scripts that run on multiple platforms using npm script.

#### [cross-env](https://github.com/kentcdodds/cross-env)
This is a module for easily setting environment variables.

How to define environment variables is subtly different between Linux and Windows. This is used to absorb the difference.
However, there is nothing to set except for `NODE_ENV` and` NODE_PATH`.

#### [npm-run-all](https://github.com/mysticatea/npm-run-all)
This is a module that can call other npm scripts collectively in npm script and move multiple npm scripts in parallel as separate processes.

Since there is this module, it is no exaggeration to say that you can do your best with the npm script.

For example, you can use like this.

```
{
  "Name": "sample",
  "scripts": {
    "compile": "run-p compile: *",
    "compile: main": "cross-env NODE_ENV = production webpack --profile - config config / webpack.main.js",
    "compile: renderer": "cross-env NODE_ENV = production webpack --profile - config config / webpack.renderer.js",
  }
}
```

When this is run with `yarn compile`, the` compile` task has `run-p compile: *`, so `compile: main` and` compile: renderer` are executed at the same time.

#### [rimraf](https://github.com/isaacs/rimraf)
This is a module that can delete all files and directories in the specified directory.

This is also used to absorb the difference between Linux and Windows.

## About the module bundle
I do not use the task runner, but use the module bundle. The module bundle in JavaScript is a linker to the C compiler.

Javascript divided into pieces according to roles, and responsibility ranges are compiled into [Babel] and converted to work on ES 5.
At this point, each file size increases slightly, but there is no change in the number of files.
You may think that moving simply by joining the resulting files; it is not.

There are other CSS meta-languages ​​that do not work unless compiled. We also compile meta-languages ​​such as Sass and Less to CSS as well.
In the meta-language of CSS, files are often combined into one at compile time.

If you use [CSS Modules], you also need to compile CSS and JavaScript with consistency.

By integrating these compiled resources into the bootstrap entry HTML, the JavaScript application will be a working module bundle.

### How to choose a module bundle
The candidate module bundle is [Browserify](http://browserify.org/), [webpack], [Rollup](http://rollupjs.org/), [Backpack](https://github.com/Palmerhq/backpack)
There are several. However, there are no options other than [webpack].

Because I use [CSS Modules] in my project.

In addition, it is strongly dependent on [webpack-dev-server](https://github.com/webpack/webpack-dev-server) [React Hot Loader](https://github.com/gaearon/react-hot-loader).

[Webpack 2.2 has been officially released](https://medium.com/webpack/webpack-2-2-the-final-release-76c3d43bf144#.suepi 2729)
I have not used β yet because I can not get β of [extract-text-webpack-plugin](https://github.com/webpack/extract-text-webpack-plugin/releases).

There is a pretty polite [migration document](https://webpack.js.org/guides/migrating/), so you can move on if you like that.

### [webpack] related modules
In the first place, [webpack] needs to learn carefully with a fairly large application. So, I do not think to add too much.

#### [webpack-dev-server](https://webpack.github.io/docs/webpack-dev-server.html)
This is a server for running the build products of [webpack] in a simplified manner.

Since it operates based on the configuration file for [webpack], you can move applications more easily than simple server like [Express](http://expressjs.com/).

Since it has abundant functions such as being able to move as a proxy, it is convenient to read all the documents one by one.

By the way, since the web browser handles `file://` and `localhost` specially, when writing an application that uses a lot of new functions of the browser, assign a proper host name and move it.

#### [webpack-merge](https://github.com/survivejs/webpack-merge)
The [webpack] configuration file will create objects every `NODE_ENV`.
If you describe the same parts in different environments and parts different for each environment, you need to synthesize the configuration objects.

It is this module that can be used at that time. I do not have to use it since it is not particularly advanced.

#### [webpack-validator](https://github.com/js-dxtools/webpack-validator)
It is a module that can check whether the configuration file of [webpack] is written correctly.

In webpack 2, similar features are implemented, so these modules are not needed.

# Topics on [Electron]
[Electron] is a framework for creating GUI applications that run on multiple operating systems. Internally [Node.js] and [Chromium] move.

[Electron] operates on a rich model derived from Chrome. To talk about it, if you do not have threads and want to do that, start the process of [Node.js]. Processes are started as tabs or windows.

In models that start more and more processes, there is only interprocess communication to share the state. In other words, complicated bugs like thread contention do not occur. Instead, it consumes a lot of memory.

There are various toolkits of this kind from the past. [JavaFX](http://www.oracle.com/technetwork/jp/java/javafx/overview/index.html), [Qt](https://www.qt.io/) something like that.

On the other hand, the advantage of [Electron] is that you can create GUI applications with a knowledge set to write web applications.

## Why to use Electron
For me it's great that [VS Code] is implemented with [Electron].

My latest work by [Erich Gamma](https://github.com/egamma) who designed the Eclipse plug-in framework that dedicated my 20's is [VS Code].
For over a decade, I thought he was a god almighty, but reading a bad code of [vscode-tslint](https://github.com/Microsoft/vscode-tslint) a little closer I got it.

[Erich Gamma](https://github.com/egamma) The teacher has a great achievement as an architect, but as a programmer, he is an ordinary man or less.
The code is copying the poor quality from that side, and there is no decent unit test. It's small in size, and I am working almost entirely, so it's a bit nicely done, is not it? Well, that's right.

I think it was wonderful of the GitHub era that such things came to be casual.

Returning the story, other applications that are being used everyday [Electron] include [GitKraken](https://www.gitkraken.com/) and [Curse](https://www.curse.com/), [Insomnia](https://insomnia.rest/), [Marp](https://yhatt.github.io/marp/).
Slack's Windows client is also [Electron], but I do not use it because it is not particularly user-friendly. Although it is a serpent, Kindle's Windows client is Qt.

Applications that UIs are far from the OS native ones used to be hated.
Recently OS native UI and web application UI are gradually approaching, so [Electron] based application releases less discomfort.

The last reason to tackle [Electron] is that the build process for creating execution binaries that run on each OS is easy.

[electron-builder](https://github.com/electron-userland/electron-builder) If you write a little bit in the package.json and build it with a little bit on the CI server, the executable binary with installer will pop up Come out.

# Summary
At last, I'm going to develop the application I wanted to make.

Originally it was supposed to be the first prototype in the New Year holidays, but failed to estimate whether the arm is dull, now the development environment is in place.

I learned so many things, some sorted out with reasons attached.

When making an application, there is something like pains of birth, and consciousness turns to the option not taken to escape from it.
You forget about what you truncated and why, you want to waste your time.

When I become myself in such a state of mind, I will be able to reconsider just by a little going, if my entry comes out. For that purpose, I wrote this entry.

When I discuss with experts on application development with JavaScript, this entry would be a criterion for matching situation recognition.

If you read such a long entry to the end, you have only words of gratitude. Thank you very much, and cheers for good work.

[VS Code]: https://code.visualstudio.com/
[Babel]: https://babeljs.io/
[Google Closure Compiler]: https://developers.google.com/closure/compiler/
[DefinitelyTyped]: https://github.com/DefinitelyTyped/DefinitelyTyped
[TypeScript]: https://www.typescriptlang.org/
[Flow]: https://flowtype.org/
[flow-typed]: https://github.com/flowtype/flow-typed
[ESLint]: http://eslint.org/
[AVA]: https://github.com/avajs/ava
[power-assert]: https://github.com/power-assert-js/power-assert
[Yarn]: https://yarnpkg.com/
[CircleCI]: https://circleci.com/
[AppVeyor]: https://www.appveyor.com/
[gulp]: https://github.com/gulpjs/gulp
[Grunt]: http://gruntjs.com/
[React]: https://facebook.github.io/react/
[Angular]: https://angular.io/
[Vue.js]: https://vuejs.org/
[Mithril]: http://mithril.js.org/
[CSS Modules]: https://github.com/css-modules/css-modules
[react-css-modules]: https://github.com/gajus/react-css-modules
[babel-plugin-react-css-modules]: https://github.com/gajus/babel-plugin-react-css-modules
[Flux]: https://facebook.github.io/flux/
[Redux]: https://github.com/reactjs/redux
[redux-saga]: https://github.com/redux-saga/redux-saga
[redux-observable]: https://github.com/redux-observable/redux-observable
[RxJS]: https://github.com/Reactive-Extensions/RxJS
[PostCSS]: http://postcss.org/
[webpack]: https://webpack.github.io/
[css-loader]: https://github.com/webpack/css-loader
[Electron]: http://electron.atom.io/
[Node.js]: https://nodejs.org
[Chromium]: https://www.chromium.org/Home
