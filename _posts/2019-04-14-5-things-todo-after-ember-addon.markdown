---
layout: post
current: post
cover:  /assets/images/ember-addon/tomster-hat-1.png
navigation: True
title: 5 Things to do after building an Ember Addon
date: 2019-04-14 00:06:43
tags: ember-addon
class: post-template
subclass: 'post tag-ember-addon'
author: robin
---

Congratulation on building your ember addon. But the journey is not done yet. Don't you want your addon to be super easy to maintain and have a lot of collaborators? **Follow this 5 steps to get you ember addon to the Next level:**

### 1. Writing docs for your Addon
<br>
Having an addon that solves a problem is not good enough sometimes, you need to have good documentation that tells how to use your addon.

> Good documentation is always necessary for making your addon a success.

These days, a well-maintained Ember addon should:
  - **Provide interactive demos of their components** in the context of an Ember app
  - **Show current and versioned guides,** ideally whose content is verified by automated tests
  - **Show current and versioned API documentation** derived from structured comments in source code
  - **Make it easy for contributors to correct documentation errors** in addition to submitting code fixes

Thankfully we have [ember-cli-addon-docs](https://github.com/ember-learn/ember-cli-addon-docs) that solves all these documentation problems

It is easy to set-up and gives an awesome out of box layout for your interactive docs.

You can even have your docs along-side you components, _so your docs always remain updated_. How about that.

### 2. Having test cases and good test coverage
<br>
Your addon may be working fine for now. But what after you have merged a couple of pull-requests. Did any of those mess up any existing functionality?

Only way to test that is to have test cases and run them with each pull-request branch. That way you can be sure your addon's functionality stays intact even after a new feature is added. This is all the more necessary for an open source addon.

Ember has a robust and easy to learn [test framework(Qunit)](https://guides.emberjs.com/release/testing/) installed by default that makes writing tests a breeze.

You can also use the interactive docs page from addon-docs to write your test cases on top.

[ember-cli-code-coverage](https://github.com/kategengler/ember-cli-code-coverage) is a good tool to get code coverage reports for your code.

### 3. Speed up your test case runs
<br>
Well, you have written your test case and they run fine. But wouldn't you love it if we could make **tests run faster**... way faster. This becomes more use-full in step 4.

Introducing [ember-exam](https://github.com/ember-cli/ember-exam)

It provides the ability to randomise, split, parallelise, and load-balance your test suite by adding a more robust CLI command.

### 4. Running tests for different ember versions
<br>
Don't you want your addon to **work with different ember versions**, and how do you test with different ember versions?

[ember-try](https://github.com/ember-cli/ember-try) is the answer to that.

ember can run tests against multiple bower and npm dependencies, such as ember and ember-data.

Coupled with ember exam can make ember try run faster.

This way you can be sure your addon supports ember versions say.. 2.8 - 3.9 just by running the ember try command.

ember try can also be used to run against other dependencies versions as well say, ember-data.

### 5. Having a CI system
When pull-requests pour in for you open source addon, manually running test cases against them can be a pain in the ass.

Thankfully there are good CI systems that run your test and show the results all inside GitHub.

Some good CI systems are [travisci](https://travis-ci.org), [circleci](https://circleci.com/) and both provide free plans to start.

Ember by default comes with travis config files, so you can start using travis out of the box if you wish.

There is also [jenkins](https://jenkins.io/) through which you could build your custom ci server. But you would have to maintain that also !!

---
 You are now ready to open your addon to the wild, I mean the internet of-course
.

 There are many other small things you can do to make your addon life easier. But there are the 5 steps I found should be done first.

**What do you think, any other things to do, after building your ember addon?**
