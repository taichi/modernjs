I made a repository that organized the learning outcomes for this month, so I will summarize the results.

If you want to easily create only the sample project you just created, please clone this repository.

* [taichi/js-boilerplate](https://github.com/taichi/js-boilerplate)
  * The master branch contains a minimal JavaScript development environment with sample code
  * The frontend branch contains development environments for [React] / [Redux] / [webpack] web applications
  * In the electron branch that is the default branch, in addition to the contents of the frontend branch, an environment for developing applications with [Electron] is included

# Introduction

## About Modern JavaScript
I'm not writing JavaScript as a job, but in the last six months I have made some small tools. Both of them are usefull, so please use it by all means.

* [ci-yarn-upgrade](https://github.com/taichi/ci-yarn-upgrade)
  * A tool that automatically creates PullRequest if there is a dependency library update in a project using [Yarn]
* [vscode-textlint](https://github.com/taichi/vscode-textlint)
  * [textlint](https://github.com/textlint/textlint) extension for [VS Code]

That's why I am not chasing the cutting-edge JavaScript, but I do not know at all.

In my current JavaScript, since various libraries and tools are going out or disappearing, it seems like there is movement, so what I want to do and things I can do are not increasing so much It is.

The content I wrote here is as much as the quantity, as I picked up as much movement as possible for about three years from the past two years. It will change a lot if you fall for three years.

## Main problem areas in JavaScript

From now on, I will explain the knowledge about JavaScript I recently learned, but list the areas to be discussed.

This is the list of "I wanted to do things and things I got able to do" written earlier.

* Topics on language
  * Introduction of type system
  * Development of static analysis
* Topics on testing
* Topics on Advanced UI
* Topics on Build
  * Management of dependent modules
  * Module bundle
* Topics on Electron

## About target audience

The target audience of this document is supposed to be a programmer who has a first language that is fully usable. The language is preferably a language of compiling process like Java or C #.

In addition, although I'm interested in recent JavaScript, those who want document to be a guide for efficient learning.

It is not a sentence with consideration for those who can not write much code or those who are not interested in JavaScript.

Those who are mastering advanced JavaScript are not subject readers, but it is nice if you point out something strange or something misunderstood easily through Twitter or Twitter if appropriate.

## Development environment
I use Windows 10 as an OS.

Since there are few platform dependencies in this story, no matter what OS you use, it does not matter, just in case.

Node should be installed under [nvm-windows](https://github.com/coreybutler/nvm-windows).
I like it because there is less trouble than [nodist](https://github.com/marcelklehr/nodist).
For Mac, I think that it is good to use [nvm](https://github.com/creationix/nvm), I do not know.

After installing Node, please install [Yarn](https://yarnpkg.com/) as well. Even if you do not know what this is, there is no problem as it will be explained later.

As an editor, I built the environment on the premise that I use [VS Code].
However, since we do not use extensions like [VS Code], it is not difficult to reproduce the same environment with other editors.

The `.vscode/extensions.json` directly under the project has the ID of the VS Code extension listed in the file.
Open the directory where you `git clone` the repository with VS Code and approve the confirmation dialog, all required extensions are installed automatically.

I use Chrome as a browser to check the operation of the application.

### Hardware specification
The specifications of the machine I use are as follows.

* Thinkpad X1 Carbon 2016 model
* CPU i 7 6600 U @ 2.6 GHz
* Memory 16 GB
* Storage SAMSUNG NVMe SSD 950 PRO 512 GB

It may be a little expensive, but it's normal for developers.

## About my background
My career started from a place where I spent about 10 years as a soldier who makes goliologie in Java with a contracted system with SIer.
So, there are a lot of knowledge about Java, the most useful thing is server side technology related to Java.

Now I can program with various languages ​​as it is, but in the end I am converting the mysterious oleo language that closely resembles Java to the brain into Ruby and Python and output it.

That's why when learning environments other than Java, there is a tendency to want to work in an environment that is easy to convert to that oleore Java.

In addition, since I am carrying on a carrier with SIer and still working at SIer, I am keenly interested in how to use this technology in contract development system development and package development.

In other words, I tend to think about how to apply new technology on the premise that technology and its improvement are not tied to revenue.

Please understand that this entry has such bias.

# Topics on Language
There are various execution environments of JavaScript, but ES5 works with most browsers.
If you do your best on ES5 in the first place, you can talk about how confusing trans-pyraes are, that is, [Babel].

As JavaScript has many knowledge to learn, first think about options not to associate with [Babel].

It was impossible for me to choose options not to go with [Babel]. The reason is simple, I do not want to write a lot of `function`. How about everyone?

In addition, if I came from Java it is true that compiling does not have a sense of evil against work.

## [Babel] is everyone's sandbox

Designing a programming language is somewhat lonely work. Until now, we have released a few language designers carefully until they are shaped to some extent.

However, at least with respect to JavaScript, it is known by ECMAScript 4's failure that it will not work in such a way.

So you can casually implement experimental language features in the form of plugins [Babel]. Discuss the function of [New language function](https://github.com/tc39/proposals) while using Gashagashi and decide the function to import into the next version of JavaScript.

Users can just try writing a cool new feature by just writing `.babelrc`. It is not enough that the function thought or it is only a matter of stopping using it if the motion is unstable.

It is fairly fun to add and subtract language features according to your needs.

### [Babel] Related understanding modules
There are many modules related to [Babel], but there are surprisingly few good ones that you should understand as chitin.

The first two are modules for setting up the environment and the other two are modules for working with other tools.

#### [babel-preset-env](https://github.com/babel/babel-preset-env)
`Babel-preset-env` is a handy module that automatically selects the [Babel] plugin to use, in conjunction with the environment where JavaScript converted by [Babel] operates.

As JavaScript improves the execution environment of browsers and [Node.js] and so on, the plug-in of [Babel] required at compile time changes accordingly.
Compilation time is only slightly increased where unnecessary plugins are activated, so it is not very harmful.

However, it is not very healthy not to keep up with the latest state unless you are conscious.

Using `babel-preset-env` will be freed from such bold work. Just updating the module on a regular basis will bring you the latest environment. Great.

#### [babel-register](https://github.com/babel/babel/tree/master/packages/babel-register)
`Babel-register` is a hacking module that hooks Node's` require` and inserts [Babel]'s processing.

It is not used for loading modules with production code, but mainly for running test code.

When executing unit test code in JavaScript, it is common to execute a test from detection of change of file.

At this time just modifying the test code slightly, if you compile all the project code, it will not work at all.

If you want to fully automate the process of compiling only files that changed and files related to them, you can hook `require` or` import`.

Please note that the test code does not refer directly to the code under test, such as a test like E2E test, it does not work very well.

#### [babel-eslint](https://github.com/babel/babel-eslint)
`Babel-eslint` is a parser for handling JavaScript code extended with [Babel] with [ESLint].

[ESLint] has evolved properly with [ESLint], so if you normally write code on ES and JSX you do not need `babel-eslint`.

Why is this necessary because you use a tool to declare and validate the type in JavaScript called [Flow].

As for [Flow] and [ESLint], I will explain properly later, so I want you to feel secure.

#### [babel-plugin-transform-flow-strip-types](https://github.com/babel/babel/tree/master/packages/babel-plugin-transform-flow-strip-types)
`Babel-plugin-transform-flow-strip-types` is a module that automatically removes information about types declared for [Flow] as its name suggests.

After deleting the information on the type, it converts the written code to JavaScript executable at the target runtime.

If [Babel] runs with only a hardened language specification without using [Flow], almost no configuration is required.
So, I wonder not to repel [Babel] so much.

## About the type system
Even if it is a type, nothing is going to talk about such a difficult story. It is a story of trying to make a bit of missing JavaScript's missing taste.

Since JavaScript has almost no type declaration in the first place, as a simple story it is absolutely unknown if you just look at the code of the called side to see what comes up in the function's argument.

If you write both the caller and the called party in your own time in a short period of time, you know exactly what to pass as an argument.
Then, there is a story that it does not bother to declare it.

But I want you to think calmly. Are you yourself a week ago? I can say that it is different. Even though I know roughly what last week's I thought about writing the code, it is an honest point that I do not know in detail.

It is not a bad idea to provide information to others including yourself in the future by writing comments in the code, but comments do not work and you can not validate automatically.
On the other hand, if you write the type as a little memo, you can automatically verify and be happy.

### Faction of type declaration in JavaScript
Let's check out how to declare types in JavaScript around here a little.

#### [Google Closure Compiler]
The first time I worked on a type declaration while using JavaScript is [Google Closure Compiler's Type System](https://github.com/google/closure-compiler/wiki/Types-in-the-Closure-Type -System).

When he writes a type declaration briefly in the document comment, he is a companion checking it.

```
/ **
 * Some class, initialized with an optional value.
 * @ Param {! Object =} opt_value Some value (optional).
 * @constructor
 * /
Function MyClass (opt_value) {
  / **
   * Some value.
   * @ Priate {! Object | undefined}
   * /
  This.myValue_ = opt_value;
}
```

What makes this approach wonderful is that the type declaration is in the comment so that the programmer can provide the necessary information by the compiler without affecting the behavior of the source code at all.

On the other hand, there is a problem that the declaration in the comment is easy to be ignored. There is also a problem that expression power is extremely low instead of being easy to understand.

'ADVANCED_OPTIMIZATIONS' of [Google Closure Compiler] has a risk that the code will not move at all, but I was really surprised because it's seriously smaller.

#### [TypeScript]
[TypeScript] appeared as one of a lot of efforts to develop a new language as an advanced JavaScript.

For me, I think that [TypeScript] is a kind of grace from God, with the recognition that Professor Anders Hejlsberg (https://github.com/ahejlsberg) who designed Delphi and C # has the latest work.

Let's look at the code of [TypeScript] a bit.

```
Interface Person {
    FirstName: string;
    LastName: string;
}

Function greeter (person: Person) {
    Return "Hello," + person.firstName + "" + person.lastName;
}

Var user = {firstName: "Jane", lastName: "User"};

Document.body.innerHTML = greeter (user);
```
It is [TypeScript] that is similar to the original Java in oleore Java in my brain.

The type declaration of [TypeScript] is provided with a way to mix type annotations in the code to work and a way to create a special file for type declaration.
For example, if you create a special file, declare it like this.

```
Declare namespace myLib {
    Function makeGreeting (s: string): string;
    Let numberOfGreetings: number;
}
```

If you have a file called `d.ts` for type declaration, you can write the code as if there is a type in a library with no existing type declaration.

It is a very important function for utilizing existing assets to be able to type in post-installation as needed for existing libraries without type declarations.

The [DefinitelyTyped] project that was working to collect type declaration files for OSS modules into a single repository was overwhelmed by too many type declaration files.

Now we are managing it in an organization called [types](https://github.com/types) in a straightforward way to allocate one repository for one module.
Nonetheless, [DefinitelyTyped] is not referenced.

By the way, in order to retrieve the type declaration file from the npm repository, you can do `npm install - D @ types / lodash` etc .. It is not a dedicated tool, it's great to get it in npm.

#### [Flow]
And [Flow]. This is a static analysis tool made with OCaml, and it checks the type declaration mixed in JavaScript and it performs an arecore check.

Let's see a bit of JavaScript code typed in [Flow].

```
// @ flow
Function total (numbers: Array <number>) {
  Var result = 0;
  For (var i = 0; i <numbers.length; i ++) {
    Result + = numbers [i];
  }
  Return result;
}

Total ([1, 2, 3, 4]);
```

It looks like it is similar to typeScript's type annotation. But [Flow] has a distinctly different point from [TypeScript].
[Flow] does static analysis such as evaluating type annotation given to JavaScript, but does not create a new programming language.

It is a closer approach to [Google Closure Compiler]. However, unlike the time when [Google Closure Compiler] was created, there is now [Babel], so once you have extended the syntax of the language primarily and you finish using it, you can safely remove only the description specifically for [Flow] Implementation became possible.

Type declarations done in comments, expression power will definitely be limited. [Flow] got over it. Nevertheless, compared with Haskell and OCaml, it does not have much rich expressiveness, even compared to Java.

It is not so difficult to understand the extent to which [Flow] infer the type. Or [Flow] does only very simple type inference.

In [Flow] there is a repository called [flow-typed] for sharing type declaration like [TypeScript].
I do not know why I'm going to do the same thing as [Definitely Typed], but I am doing it in a single repository.

However, this seems to be reviewed severely when throwing out PR, and the contents of the stored type definition file are firm, instead of being absolute.

When retrieving a type declaration file, using the command `flow-typed` will automatically download the dependency module type declaration file in package.json from the [flow-typed] repository.
For those without a type declaration, we make a type declaration file that all types declare any.

Why will not you make it possible to take type declaration files with the npm command? It is not a good idea to separate the commands that make a miscellaneous type declaration file separately.

### Which type system to use
There is no [Google Closure Compiler] at this time in shimashi.

Since [TypeScript] and [Flow] are different objectives, they are not something that can simply be evaluated in the star table. If you compare it in the Star table, [TypeScript] is a victory.

Well then, which one should you use?

It would be nice to look at the correspondence situation of the library and framework you want to use.

If you use at least [Angular] it will be [TypeScript] or [React] if you use [Flow] you are certain.

[There is an official type definition file for TypeScript in Vue.js](https://vuejs.org/v2/guide/typescript.html). However, this does not recommend using [TypeScript], it seems that the contribution by some users was merged.

If you use elementary JavaScript, it is good to use [Flow], and [TypeScript] is a pretty good choice if you have the type definition files of the library you'd like to use.
Especially [VS Code] clearly improves the precision of input completion if the type definition file is complete.

Supplementally, [Flow Language Support](https://marketplace.visualstudio.com/items?itemName=flowtype.flow-for-vscode) also has a function to enhance input completion.

## Unhandled task on the type system
Supplement the indications received from experts after this entry release.

* [Dart](https://www.dartlang.org/)
  * It is a language that makes strong type inference than [TypeScript]
  * [Dart2js](https://webdev.dartlang.org/tools/dart2js) can convert it to JavaScript
  * [Angular] Within 2 Google's [large case already exists](http://news.dartlang.org/2016/03/the-new-adwords-ui-uses-dart-we-asked. Html)
  * Large-scale case is due to [Angular 2 Dart](https://github.com/dart-lang/angular2)
 
## Let's share best practices with static analysis
JavaScript has many traps. The trap here is the language specification that many programmers tend to misunderstand, and the behavior of certain libraries.
Because JavaScript is a truly flexible language, most of the new language specifications can be converted directly into the old language. That is why there are many traps.

Code written as a result of mistake by programmer or code not written unless misunderstanding about specifications. A code that can be considered a bug in most situations.
Although it conforms to the language specification, Lint is a tool for automatically finding the code which can not obtain the expected results even if it is operated as an application.

If you use Lint a bit more aggressively, you can inspect anything that has nothing to do with the behavior of the code, such as how to indent and how to name variables, but also on code readability.

### History of Lint in JavaScript
Javascript looks a bit and there are lots of Lint. The one oldest that I have used is [JSLint](http://www.jslint.com/).
This guy is really hard at the Lint made by Professor [Douglas Crockford](https://github.com/douglascrockford) of the legend. It's a strong strong style that does not allow any indulgence.
It is painful for hetare because it is a tool that feels a strong will that code that can do strange behavior should be a strange look that is never misled by strange behavior of JavaScript.

By the way, [reading the code of JSLint](https://github.com/douglascrockford/JSLint) is a great learning experience. It's compact and easy to read.

Lint that became evangelical for a hetare like me is [JSHint](http://jshint.com/). This is not too strict. It is extremely easy to use because it can cut out the setting file.
However, JSHint is easy to enable and disable the built-in functions, but adding a new rule is a little troublesome.

Also, in order to incorporate someone's recommended rules into your project, you have to copy and paste the settings.
Most people want to use someone's recommendation set as a part of maniacs like me like to make Lint's rules.
If that recommended set is maintained, you want to use only that final result without doing troublesome things.

That's why it's easy to extend and easy to create configuration files [ESLint] is now recommended.

[ESLint] is a good plug-in system, so there are plenty of plug-ins and you can publish the recommended rule set as npm module.

Airbnb's published [eslint-config-airbnb](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb) is one such kind of recommended rule set.

Airbnb contains a lot of things that do not fit my idea, so I did not adopt it.

### [ESlint] Related understanding modules
Speaking of [ESLint] related modules, it's about plug-ins.

I think there are many other things, but list those that I found particularly useful among those I found and used.

If you are an [ESLint] mania and I know a useful plugin I do not know, please let me know.

#### [eslint-plugin-ava](https://github.com/avajs/eslint-plugin-ava)
In my project I use a testing framework called [AVA]. [AVA] will be explained later.

[AVA] is fairly simple and easy to use, but it is too simple to understand until you get used to how to write the test code.

So, using this plugin makes it possible to warn you [AVA] obviously when using incorrect usage.

#### [eslint-plugin-import](https://github.com/benmosher/eslint-plugin-import)
`Eslint-plugin-import` finds errors on` import` statements.

Specifically, if you try to `import` a module that does not exist in the project you will get a warning. Everyone can be typoing the module name.

In addition to this, if you pass a string other than literals to `require`, you get an error.

#### [eslint-import-resolver-node](https://github.com/benmosher/eslint-plugin-import/tree/master/resolvers/node)
Only this one has a different role from other modules.

`Eslint-import-resolver-node` can work with` eslint-plugin-import` to customize how to find `import` modules.

As for what I mean, in my project I have stored application code and test code in separate directories `src` and` test` respectively.

On this test code side, when you do `import` application code, it is very painful to write` import world from '../../ src / hello / world ";`
In order to avoid it, by setting `NODE_PATH = src` when executing the test code, we do not need to write that` ../../ src / `part.

It uses `eslint-import-resolver-node` to inform` eslint-plugin-import` about this.

#### [eslint-plugin-promise](https://github.com/xjamundx/eslint-plugin-promise)
The JavaScript `Promise` is a type of library you need to get used to. Moreover, it is hard to understand the correct usage.
By the way, [JavaScript Promise's book](http://azu.github.io/promises-book/) is a very wonderful document so I can read it many times.

It is highly likely that you are using the wrong `Promise` until you get used to` Promise`.
If there are JavaScript experts in the vicinity and can receive code reviews, it is very wonderful, although I am writing a bit of JavaScript as a hobby, I can not call such an expert.

So by using this plug-in, you will get an error in the obviously bad `Promise` code.
You can understand how to use `Promise` by simply reading the rule details of this plugin, so please set the rules carefully.

By the way, if `async / await` enters ES, will not you use` Promise` directly?

I think that `async / await` and` Promise` coexist normally with the experience of using C # 'async / await`, but how about it actually?
At least, using [async / await] in [AVA] has the impression that the test code is easy to understand.

#### [eslint-plugin-security](https://github.com/nodesecurity/eslint-plugin-security)
Do you care about security while writing code?
I am doing security reviews using static analysis tools as a job, so I am going to care about it as it is.
However, whenever it is said that there is consideration on security in the head, it is not such a thing.

The vulnerability that can be checked with this plug-in is not much, but it finds a vulnerability that will make it easy to make a hit.

As long as I write the code normally, I do not see any errors due to this plugin, but when I get angry with this plug-in, I want you to be a little cautious.

#### [eslint-plugin-flowtype](https://github.com/gajus/eslint-plugin-flowtype)
Although it is good to type declaration using [Flow], there are things that make a huge mistake.

Also, since the type declaration by [Flow] is not ordinary JavaScript, the standard rule in [ESLint] is not applied at all.
In other words, you need to reimplement it for [Flow], indentation, sticking a semicolon at the end of the line, or killing Quetz kama.

[Flow] itself has its own traps, and if you declare a consistent type declaration, it would be better to keep it a bit Lint.

Although we did the same story even at `Promise`, it is good to use` eslint-plugin-flowtype` as a training Gibbs until [Flow] is also a new technology.
If you get used to it properly, the error will not come out completely.

#### [eslint-plugin-react](https://github.com/yannickcr/eslint-plugin-react)
In my project [React] is used as a framework for building UI.

Since [React] is very commonly used, along with many best practices, undesirable coding styles have been found.

Everything defined as a rule is not always desirable for our project, but it is much easier to argue about what is being ruleized than to create that knowledge.

In fact, in order to get knowledge about the rules as defined in this plug-in by yourself, it will not be enough to simply use it with a small project.

The merit of using a de facto standard framework lies in this place.

#### [eslint-plugin-jsx-a11y](https://github.com/evcohen/eslint-plugin-jsx-a11y)
How careful are you using the `Accessibility` of the system that we are making?

I feel that it is `Accessibility` that has a lower priority than Security which tends to be negligible.

Because it is to provide a rich UI for a better user experience, it may be good to give consideration to users with handicapped on the extension line.

However [WAI-ARIA 1.1](https://www.w3.org/TR/2016/CR-wai-aria-1.1-20161027/) and [MDN's ARIA page](https: / /developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA) from end to end and it's impossible to say you're going to do it.

[HTML5 Accessibility](http://www.html5accessibility.com/) I would like to provide maximum value with minimal effort to one easy handy reference hand.

That's why, let's first make sure that the JSX in the project uses the `eslint-plugin-jsx-a11y` recommended rule to deal with the` Accessibility` correspondence.

# Test talk
Even if it says a test in a bite there are various. Here we talk about developer testing.

I think the technology that the programmer should master the most in application creation is test.
It is because we believe that by expressing what it is supposed to be in the form of tests, better ones can be made by implementing the target software.
In other words, I do not want to talk that I should be able to do something that a real test engineer is doing.

For quality assurance there is enormous systematized knowledge, knowledge to make applications, both wonderful, but the time is finite.
There are families or games that I want to do. I want to sleep ten hours a day to live long, I want to eat delicious meals slowly.

You should spare time learning what to test and what to test, so do not take time to learn how to use the testing framework coolly.

That's why the testing framework chooses the API that is as simple as possible.

That is, it is [AVA].

## Recommendation [AVA]

The test code written in [AVA] is like this.

```
Import test from 'ava';

Test ('foo', t => {
    T.pass ();
});

Test ('bar', async t => {
    Const bar = Promise.resolve ('bar');

    T.is (await bar, 'bar');
});
```

Let me briefly list why I like [AVA].

* API is small
* [Power-assert] is built as standard
* Faster execution speed
* Perfect support for the `=>` operator, `Promise`,`async/await`, [observable](https://github.com/tc39/proposal-observable)

### [AVA] API is small
After importing with `import test from 'ava';` you can do whatever you want by calling the function defined in the `test` variable.

And even if this `test` is a default function, there are only 11 functions. Let's enumerate

* ** test ([title], implementation) **
* Test.serial ([title], implementation)
* Test.cb ([title], implementation)
* Test.only ([title], implementation)
* Test.skip ([title], implementation)
* ** test.todo (title) **
* Test.failing ([title], implementation)
* Test.before ([title], implementation)
* Test.after ([title], implementation)
* ** test.beforeEach ([title], implementation) **
* ** test.afterEach ([title], implementation) **

The only thing to keep in mind is the four highlighted APIs. Other than that, you can refer to the manual when it becomes necessary.

It is also important that [AVA] does not work on global space or functions placed in implicit `this`.

### [power-assert] is included as standard
When writing tests, use [power-assert] which will give you the richest assertion error if you use it without thinking anything.

This test is ...
```
Test (t => {
    Const a = / foo /;
    Const b = 'bar';
    Const c = 'baz';
    T.true (a.test (b) || b === c);
});
```

This is an error.
```
T.true (a.test (b) || b === c)
       | | | |
       | "Bar" "bar" "baz"
       False
```
Awesome!

[Power-assert] rewrites the code to issue this assertion error, but the setup for that is a little troublesome. It is easy to do and manuals are available, but well, it is troublesome to have trouble.

But, if you use [AVA], it does not even have to be a little troublesome.

### Faster execution speed
The speed at which the test is executed becomes a problem after releasing a real application properly and entering the maintenance phase.

While running for a while, the execution speed will be 5 seconds, but it will be 1 second, unchanged. But what if the amount of test code is 1,000 or 10,000?

It is no longer possible to run all the tests on your local machine each time you change the code.

Just run the test code that seems to be directly related to the part you changed, and then just throw it to a CI server like Jenkins or CircleCI and pray. I wish for not knowing about it. However, it gets mocked.

Since the test is always mocked regularly, it is better to shorten the cycle of change → test → debug → change ......

[AVA] is designed not to dare to write a test code of a very complicated structure in order to secure the test execution speed.
From a person who wrote a complicated structure test with a testing framework like [Mocha](https://mochajs.org/), it may seem strange, but it is familiar.

Variable scope of test code is accumulated many times, it is good when writing, but after all it will be a hard time.

### Perfect support
This is only a talk that the API of [AVA] can be perfect as it is simple.

Since `it` is not rewritten badly, the` => `operator moves comfortably.

Since the return value of the test method is determined as the responsibility of [AVA], whether it is `Promise`, but if you return it as [observable](https://github.com/tc39/proposal-observable) AVA] will do it for you.

Function calls aligned with test code files are not nested.

By the way, to reference the variable created by `test.beforeEach` with` test`, do this.

```
Test.beforeEach (t => {
    T.context = 'unicorn';
});

Test (t => {
    T.is (t.context, 'unicorn');
});
```
`context` is not shared among multiple` test`, so you can touch it altogether.
However, multiple `test` can not guarantee ordering unless` test.serial` is used, so do not modify `context` in` test`.
In the first place `test.serial` should not be used unless there are any special reasons.

Also, do not do it because it is bad either to share some information between test methods or to write in such a way that the test execution order has meaning.

## Unhandled task related to the test
I should learn, but please write a little, hoping that someone will supplement the tasks that have not been done yet.

* E2E test or system test
  * It seems that [React application can be tested] at [Protractor](http://www.protractortest.org/) (http://www.joelotter.com/2015/04/18/protractor-reactjs.html)
* Mutation test
  * [Stryker](http://stryker-mutator.github.io/) is okay but there is no [AVA] support
* stress test
  * In particular, there is a rich UI load test methodology and a tool for that, made by JS, but it should be nice, but it has not been successfully found

# How to make a UI
From here, I will explain how to construct a user interface with JavaScript.

# About choosing tools

When I used Gmail and Google Maps for the first time about 10 years ago, I was really surprised as to whether such high-performance and rich screen expression can be done with JavaScript alone.

People who are tackling things like making DHTML and using websites to move things using JavaScript had long been abhorred even though they were there before.

To run an application with a rich user interface in the browser, we had to use Flash. There were other technologies like browser plug-ins, but in the end it was rarely used.

Activities such as Gmail and GoogleMaps that implement GUI applications that work with realistic performance using JavaScript only on the browser have been active since that time, but until the last few years their efforts are very well I did not say that.

Certainly [YUI Library](http://yuilibrary.com/), [Ext.js](https://www.sencha.com/products/extjs/), [Closure Library](https://developers.google.com/closure/library/] Although libraries such as  came up one after another,
It was incredibly difficult to make a UI like it was made with VB and Delphi.

For example, it was not realistic to display grid control such that each cell performs a complicated operation as it is after displaying about 1000 data.

As a result, in addition to CSS, it became a standard to work hard on jQuery a bit. If you only want to make a website with a little movement, you can do it with jQuery.
If you do work with a large number of people, do maintenance for a long time, or do not make extraordinary things, it will generally be fine.
Most websites are not GUI applications, so you can quickly create jQuery and its plugins conveniently.

By the way, is there a clear boundary between highly-built websites and GUI applications running on browsers?

I have never seen a quantitative judgment criterion, but at least one website creator can use jQuery in one hand to create a movement without hurting, at least it will be a website.

On the other hand, three or four programmers who have server applications and can write proper JavaScript are necessary to make UI,
The UI design and its implementation that would not be established unless it is shared as a role would be a GUI application running on a browser.

Actually, it will not be easy to distinguish this so easily.

The reason why this distinction is important is that the toolkits to use and skill sets of team members to be aligned are different.

If you try to create a website using a framework for implementing sophisticated GUI applications like [React] and [Angular], you will waste a lot of manpower.

But if you are planning to implement sophisticated GUI applications using jQuery plugin, it will be ruined before that.

Tools should be used properly in appropriate places.

## Breakthrough called Virtual DOM
The task to be surely solved in making GUI applications that run on browsers is performance and usability.

Especially the problem of performance is very difficult to solve. Since it operates on the browser, all UI elements are dropped into the DOM in some form.

First, since DOM has a general-purpose tree structure, memory efficiency is terribly bad.
In addition, this tree structure can not limit the number of intermediate internal nodes, and the depth to the terminal node can not be limited, so the cost of scanning tends to be high.

In order for the browser to draw the DOM on the screen, it is necessary to scan all nodes in some form, so this poor efficiency becomes a big problem.

In addition, the mechanism of laying out drawing elements completely ignoring the DOM structure called CSS spurs poor performance.

It is natural in the browser world, such as having to redraw the entire screen with one class attribute that appeared at the terminal node.
In other words, it is necessary to lock the entire screen just by `appendChild` the newly created node for the DOM already drawn.

However, if the impact of the new node on the entire screen is small enough, the time to keep securing locks will be sufficiently short.

Just updating certain components in the screen may cause redrawing of the entire screen, so you need to pay close attention not to do so.
In such a case, except for some exceptional organizations, it is not possible to efficiently create large-scale GUI applications.

Virtual DOM is the technology that answers this problem.

First, consider the data (model) necessary for rendering the screen and the template (view) into which the data is poured.
If you add a controller that has the role of updating the model from user input to this, it becomes a classic MVC model.

In the primitive way, programmers decided which part of the view to change, depending on which of the components of the model was changed. Here is a very simple example.

Consider putting the variable `user` in the first half into the HTML-like template in the second half.

```
// Example model
Var user = {
  "Name": "John Doe",
  "Age": 22
}
// View example
<Html> <body>
<Div class = "name">
Name: $ {user.name}
</ Div>
<Div class = "age">
Age: $ {user.age}
</ Div>
</ Body> </ html>
```

Updating the model means changing the contents of `name` and` age` which are members of the variable `user`.
In the primitive way, the programmer must write code to find the corresponding DOM element, depending on which member variable you changed.
Indeed, in this simple example, you can find the corresponding element by writing `$ (". Name ")` in jQuery.

Think about it, if there are 1,000 pieces of changeable elements in the screen?
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
This is due to the relatively short period since stable releases of stable ones.

The current [Angular] is 2.4, but the first type of [Angular] is my favorite type of framework.
Anyway, there is a 2way binding, there is a Directive where every black magical act is allowed as a garbage repository, there was a DI container.
The digest loop was a very difficult concept, but I thought that complexity should be accepted in order to make the screen easier to make.

The first line of [Angular] was a framework that seems to exist for SIer in a function group that seems only to be designed for creating business applications.

I think that it is not too late to drop a large learning cost on [Angular] 2 system even after full-scale adoption case inside Google is released.

### [Vue.js]
[Vue.js] is a topical framework right now. I liked the 1 series of [Angular] The one I should choose is [Vue.js].

But, I can not adopt what is [the main developer is only one](https://github.com/vuejs/vue/graphs/contributors) to the base part.
[Evan You](https://github.com/yyx990803) It can be a catastrophic situation if one person loses a little enthusiasm, or just transforming by some political transaction.
It's a terrible thing, it's an unbearable risk to me.

Randomly as far as I can read the code base gets a reasonable impression as such and there are quite polite documentation.
It seems there is no shortage in function with the feeling that I tried a little.
However, when I thought about making a large application as appropriate, I pulled the fundamental problem in an unexpected place and thought about the possibility that I could not cope well with that.

There are large communities, and those that are used as kitsin in a live application have been discovered and carefully dealt with many fine problems.
The code which seems to have meaningless complexity at first glance may also have important meanings in certain circumstances.

This is only my bad impression theory, but the second line of [Angular] and [Vue.js] have the beauty of the invisible code base as used in a serious production environment.

By the way, the first line of [Angular] was a code base that seems to be used in production environment.

### [Mithril]
The goodness of [Mithril] is small. After fully understanding the internal details of the framework, it is a tool used by people who can add it myself by myself by noticing what is missing.

Because it is small, you can do it.

It may be suitable for use with a small team composed only of engineers with sufficient development experience of GUI applications.

It is really difficult to properly determine the specifications of those that are missing and properly implement it, assuming that they are missing while making the application.
Especially when the cost pressure during project progression is strong, can it do well? At least, I will not be able to do it.

### About [React]
Since the body of [React] has a role only for the part of the view, there are various shortcomings in making the application.

I chose [react-router](https://github.com/ReactTraining/react-router) without much trouble about Router. It can be said that I myself do not have standards for selecting Routers.

As a utility for testing, you can not remove [enzyme](https://github.com/airbnb/enzyme).
It is the greatest merit of using a de facto standard library that you can use high quality modules created by users who have incorporated frameworks.

### About [CSS Modules]
In other words, CSS is like a programming language with only global variables, a programmer who lives in a world where the default scope is local variable is an unavoidable environment.
In an environment where the complexity rises in proportion to the amount of code, I think that it is impossible to make perfectly consistent work.

What we have been dealing with this problem up to now has been to divide the namespace by explicitly defining the naming conventions by humans.
For example, [OOCSS](http://oocss.org/), [BEM](http://getbem.com/), and [SMACSS](https://smacss.com/) are included in major naming conventions is there.

Also, write CSS more securely using meta-language such as [Sass (SCSS)](http://sass-lang.com/) or [less](http://lesscss.org/) Efforts have also been made.
The most important function of these meta languages ​​is to limit the scope of influence by nesting the definitions of styles.

The meta language is excellent as a mechanism to control the complexity of the CSS itself, but since the CSS is a mechanism for deciding what kind of appearance is given to the DOM structure in the first place, the design of the DOM structure and the design of the CSS can not be separated .

If so, it is desirable that the design of the [React] component and the design of the CSS are consistent. It would be impossible to treat each as a completely independent event.

[React] In order to ensure consistency between the design of the component and the design of the CSS, either way is the priority. In that case, it is desirable to bring local variables to CSS and make it the default behavior.

The concepts and efforts devised to realize this are [CSS Modules].

[CSS Modules] is a concept derived from the community of [React], but similar functions are also found in [Vue.js](https://vue-loader.vuejs.org/en/features/css-modules.html ),
[There is a similar function in Angular 2](http://joaogarin.github.io/css-modules-angular2/) Looks like.

Detailed information on the CSS problem resolved by [CSS Modules] is [There is a blog entry by members of the CSS Modules team](http://glenmaddern.com/articles/css-modules), so please refer to that .

To summarize, [CSS Modules] is not just a kind of CSS design methodology based on new naming conventions. Also, using [CSS Modules] does not mean you do not have to use the CSS meta language.
Even web designers accustomed to existing naming conventions should learn [CSS Modules] if you are doing UI design with projects that share work with large numbers of people.

Even if you use [CSS Modules], CSS designed to make UI consistent for the entire application will be defined to the global space as before, so existing CSS It does not mean that you do not need design knowledge on the design.

### [React] in [CSS Modules]
To introduce [CSS Modules] to [React], use [css - loader] of [webpack] to process CSS and then write a dedicated description to the [React] component side. [Webpack] will be explained later.

Introduction of #### [CSS Modules]
The easiest way to use [CSS Modules] is this code.

```
import React from 'react';
import styles from './table.css';

export default class Table extends React.Component {
    render () {
        return <div className = {styles.table}>
            <div className = {styles.row}>
                <div className = {styles.cell}> A0 </div>
                <div className = {styles.cell}> B 0 </div>
            </div>
        </div>;
    }
}
```

When this is rendered, it will be roughly like this HTML. Because css class name which does not automatically duplicate by [css - loader] is named, weird name is set in class attribute.

```
<div class = "table__table ___ 32osj">
    <div class = "table__row ___ 2w27N">
        <div class = "table__cell ___ 1oVw5"> A0 </div>
        <div class = "table__cell ___ 1oVw5"> B0 </div>
    </eiv>
</eiv>
```

The problem is that importing CSS as if it is a JavaScript object is handled this way.

In this case, the CSS must also be applied to the component during unit tests that do not need to evaluate the state of the style at all.
If import is processed in a naive manner as it is, it becomes an error because it can not parse import destination CSS as JavaScript.

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
        return <div styleName = 'table'>
            <div styleName = 'row'>
                <div styleName = 'cell'> A 0 </div>
                <div styleName = 'cell'> B 0 </div>
            </div>
        </div>;
    }
}

export default CSSModules (Table, styles);
```

Instead of the braces disappearing, we now import [react-css-modules].

A special property called `styleName` has been introduced which is added to the class attribute when rendered.

Although code easiness was improved, the difficulty of the code has not improved much.

Especially, as you can see from the last line, the problem of handling imported CSS as a JavaScript object has not been solved.

#### Improvement with [babel-plugin-react-css-modules]
There is [babel - plugin - react - css - modules] as a plugin of [Babel] to solve the problem of [react - css - modules].

By the way, the developers of [react-css-modules] and [babel-plugin-react-css-modules] are the same.

Using this makes it such a code.

```
import React from 'react';
import './table.css';

export default class Table extends React.Component {
  render () {
    return <div styleName = 'table'>
      <div styleName = 'row'>
        <div styleName = 'cell'> A 0 </div>
        <div styleName = 'cell'> B 0 </div>
      </div>
    </div>;
  }
}
```

You can now avoid handling CSS as a JavaScript object. We use import statements only to show the relationship between CSS and components.

[Babel-plugin-react-css-modules] performs processing necessary as [CSS Modules] at compile time.
Thanks to that, component code does not increase only for [CSS Modules].

It is nice to have the code refreshed in this way.

Since import of CSS does not affect components on code, removing such a part with a module like [ignore-styles](https://github.com/bkonkle/ignore-styles) has no adverse effect .

So in my project I decided to use a combination of [css-loader] and [babel-plugin-react-css-modules] to implement [CSS Modules].

### About [Flux] implementation
It is good to adopt [Flux] as the architecture that defines the structure of the application.
There are many libraries implementing [Flux], but since it is popular and easy to use, [Redux] is used.

In terms of using [Redux], how much roles and responsibilities are given to Actions and ActionCreators depends on the size of the application, the optimal solution changes.

Using [reducex-thunk](https://github.com/gaearon/redux-thunk) or [redux-promise](https://github.com/acdlite/redux-promise) makes it very Although easy to understand, the ActionCreator code tends to become large.
As the code related to asynchronous processing appears in Action, measures must be taken to set some criteria and separate codes from Action in the process of increasing the code base.

In a very simplistic way, `redux-thunk` implements Action in the callback model. `Redux-promise` implements Action in the` Promise` model.

The problem with this simple approach is that you can not cancel the task.
It is common in GUI applications that there is some inconsistency between the server and the client, and users who are irritated by the response delay want to interrupt processing.
In such a case, it is not desirable that there is no way to cancel the task being processed with the user operation as the starting point.

[redux-saga] and [redux-observable] are modules that are quite difficult to understand the behavior model.
Instead, the ActionCreator code will be simpler and Action will be a fairly simple object to store the contents of the event that occurred.

[reducex-saga] and [reducex - observable] add a new layer whose name is different but has the same role to [Redux].

Again, for simplicity, [redux-saga] implements additional layers using GeneratorFunction and [redux-observable] uses [RxJS] to implement additional layers.

Both [redux-saga] and [redux-observable] have a way to cancel the task.

When writing code to handle errors, the difference between [redux-saga] and [reducex-observable] becomes clear.
For details, see the respective documents, but the point is whether to use `try/catch` or use the event definition handler of library definition.

* Redux-saga's [Error handling](https://redux-saga.github.io/redux-saga/docs/basics/ErrorHandling.html)
* RxJS [Error Handling](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/gettingstarted/errors.md)

How I tried to say, I dislike the `try/catch` syntax which is only a legal` goto`. Although I write it without any help, I do not want to write positively.

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

## Undamaged issues relating to UI
I should learn, but please write a little, hoping that someone will supplement the tasks that have not been done yet.

* Talk about usability in JavaScript GUI application
  * Talk of methodology to respond to keyboard-centered operations, operations using pointing devices other than mouse, operation by touch display, etc.
  * Talk of evaluation criteria of operability in web application
* [PostCSS]
  * In my project, I use it as [babel - plugin - react - css - modules] asks for it
  * In something like [Babel] in the CSS area, you can plug in and uninstall features by plugin ... only understand to the extent
  * [The logo that became the power-assert's logo](https://togetter.com/li/823597) and the logo of [PostCSS] are same in design direction
* [CSS Flexible Box Layout Module Level 1](https://www.w3.org/TR/css-flexbox-1/)
  * As far as we can see, the Flexible Box is the layout engine of the GUI application itself
  * The material of [CSS Flexible Box Layout](https://developer.mozilla.org/en/docs/Web/CSS/CSS_Flexible_Box_Layout) in MDN is straightforward
  * Since it is under development of specifications, it works only with the latest browser

# Build story
I talked about basic language talking, testing method, how to make UI, next is build talk.

Continuous integration (CI) is essential in contemporary development process.
In other words, you have to automate the build process with CLI. It will not change even if you develop it in the Windows environment.

SaaS of CI that I use is [CircleCI] and [AppVeyor].

[CircleCI] is used to build in Linux environment. Especially when the build fails, you can debug and collect logs by [SSH login](https://circleci.com/docs/ssh-build/).

It often happens that the build results on the CI server where the environment gets beautiful every time the environment does not match the build results of the local environment where various things are stacked.

Especially, in my case I am developing in Windows environment, so it is easy for the environment to differ from CircleCI in Linux environment. That is, SSH login is an extremely important function.

[AppVeyor] is used to build in a clean Windows environment. Just a little crafted the configuration file, you can debug and collect logs by [can connect to RDP](https://www.appveyor.com/docs/how-to/rdp-to-build-worker/).

The OS is Windows Server 2012 R2 (x64), but it does not use it so that the difference between the server OS and the client OS becomes a problem.

About managing dependency modules
Based on package.json which stores meta information about the project in Node, it comes with a tool called npm that can execute various tasks.

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
Writing a build script that runs on multiple platforms basically imposes a limitation that shell scripts can not be used.

If I have a Linux binary [PowerShell](https://github.com/PowerShell/PowerShell) you can do it, right? It is because it is a Windows user.

Since the language used in the project is JavaScript and Node works properly on multiple platforms, it is preferable to write the build script in JavaScript.

There are many modules for writing build scripts with JavaScript, but my way of thinking is simple.

Do you use [gulp] or not?

Indeed, there are task runners like [Grunt](http://gruntjs.com/) and [broccoli](https://github.com/broccolijs/broccoli).

The reason why these can not be taken into consideration is simple because it does not provide convenience that greatly exceeds npm script.
Since there are many objects to be learned at all, I do not want to pay learning costs to the task runner as much as possible.

Still, the reason why [gulp] can be considered is performance. [Gulp] is a task runner that places Node 's Stream API at the center of task construction and operates at high speed.

In the code base of hundreds of thousands of rows level, it is expected that the execution time of the build can be shortened as much as possible, as well as the test execution time should be shorter.

By the way, there is a thing called "do not predict" to what I like in the maxim of performance.
In other words, if the reason for adopting [gulp] is really only performance, its adoption is that the code base grows big enough that even if the build time is long enough it will not be late.

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
I do not use the task runner, but use the module bundle. The module bundle in JavaScript is like a linker to the C compiler.

Javascript divided into pieces according to roles and responsibility ranges are compiled into [Babel] and converted to work on ES 5.
At this point, each file size increases slightly, but there is no change in the number of files.
You may think that moving simply by joining the resulting files, it is not.

There are other CSS meta languages ​​that do not work unless compiled. We also compile meta languages ​​such as Sass and Less to CSS as well.
In the meta language of CSS, files are often combined into one at compile time.

If you use [CSS Modules], you also need to compile CSS and JavaScript with consistency.

By integrating these compiled resources into the bootstrap entry HTML, the JavaScript application will be a working module bundle.

### How to choose a module bundle
The candidate module bundle is [Browserify](http://browserify.org/), [webpack], [Rollup](http://rollupjs.org/), [Backpack](https://github.com/ Palmerhq / backpack)
There are several. However, there are no options other than [webpack].

Because I use [CSS Modules] in my project.

In addition, it is strongly dependent on [webpack-dev-server](https://github.com/webpack/webpack-dev-server) [React Hot Loader](https://github.com/ Gaearon / react-hot-loader).

[Webpack 2.2 has been officially released](https://medium.com/webpack/webpack-2-2-the-final-release-76c3d43bf144#.suepi 2729)
I have not used β yet because I can not get β of [extract-text-webpack-plugin](https://github.com/webpack/extract-text-webpack-plugin/releases).

There is a pretty polite [migration document](https://webpack.js.org/guides/migrating/), so you can move on if you like that.

### [webpack] Related understanding modules
In the first place, [webpack] needs to learn carefully with a fairly large application. So, I do not think adding too much.

#### [webpack-dev-server](https://webpack.github.io/docs/webpack-dev-server.html)
This is a server for running the build products of [webpack] in a simplified manner.

Since it operates based on the configuration file for [webpack], you can move applications more easily than simple server like [Express](http://expressjs.com/).

Since it has abundant functions such as being able to move as a proxy, it is convenient to read all the documents one by one.

By the way, since the web browser handles `file://` and `localhost` specially, when writing an application that uses a lot of new functions of the browser, assign a proper host name and move it.

#### [webpack-merge](https://github.com/survivejs/webpack-merge)
The [webpack] configuration file will create objects every `NODE_ENV`.
If you describe the same parts in different environments and parts different for each environment, you need to synthesize the configuration objects.

It is this module that can be used at that time. I do not have to use it, since it is not particularly advanced.

#### [webpack-validator](https://github.com/js-dxtools/webpack-validator)
It is a module that can check whether the configuration file of [webpack] is written correctly.

In webpack 2, equivalent functions are implemented, so these modules are not needed.

# The story of [Electron]
[Electron] is a framework for creating GUI applications that run on multiple operating systems. Internally [Node.js] and [Chromium] move.

[Electron] operates on a rich model derived from Chrome. To talk about it, if you do not have threads and want to do that, start the process of [Node.js]. Processes are started as tabs or windows.

In models that start more and more processes, there is only interprocess communication to share the state. In other words, complicated bugs like thread contention do not occur. Instead, it consumes a lot of memory.

There are various toolkits of this kind from the past. [JavaFX](http://www.oracle.com/technetwork/jp/java/javafx/overview/index.html), [Qt](https: // www. Qt.io/) something like that.

On the other hand, the advantage of [Electron] is that you can create GUI applications with a knowledge set to write web applications.

## Why to tackle Electron
For me it's great that [VS Code] is implemented with [Electron].

My latest work by [Erich Gamma](https://github.com/egamma) who designed the eclipse plug-in framework that dedicated my 20's is [VS Code].
For over a decade, I thought he was a kind of god almighty, but reading a bad code of [vscode-tslint](https://github.com/Microsoft/vscode-tslint) a little closer I got it.

[Erich Gamma](https://github.com/egamma) The teacher has a great achievement as an architect, but as a programmer he is an ordinary man or less.
The code is copying the poor quality from that side, and there is no decent unit test. It's small in size and I am working almost entirely, so it's a bit nicely done, is not it? Well, that's right.

I think it was wonderful of the GitHub era that such things came to be casual.

Returning the story, other applications that are being used everyday [Electron] include [GitKraken](https://www.gitkraken.com/) and [Curse](https: // www .curse.com /), [Insomnia](https://insomnia.rest/), [Marp](https://yhatt.github.io/marp/).
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

When I become myself in such a state of mind, I will be able to reconsider just by a little going, if my entry comes out. For that purpose I wrote this entry.

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
