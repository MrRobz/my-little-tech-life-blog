---
layout: post
current: post
cover:  /assets/images/ember-monorepo-post/dog-box.png
navigation: True
title: Splitting & Migrating your Ember project to a Monorepo
date: 2020-03-01 00:06:00
tags: Ember Monorepo
class: post-template
subclass: 'post tag-ember-addon'
author: robin
---

Let's imagine a scenario where your ember project is getting fairly large in terms of code size and is getting hard to maintain. What do you do?
You split your code into addons and ember engines right! *Monorepo is one good way to maintain these addons and engines without having to maintain multiple repos.*

##### What is a Mono Repo?
Monorepos are simply single repositories with separate npm modules organized into folders.

#### But why Mono ?
So the answer is really simple addons and engines. People love monorepos and the reason is is that, in a repository the code stays in sync with itself.
So when you have multiple modules that live within that repository, the modules stay in sync.
You can have a single PR or even a single commit that is able to orchestrate a change across multiple modules and you can also test these things together.
Also tracking issues is much more convenient when you have modules are co-located inside the same repository.

### Some advantages to having a monorepo in ember:
* Addon modules dependencies become explicit.
* Addon modules have their own test cases.
* Addon modules become publishable if you wanna share it among teams.

### Before we start: Splitting your project files into engines and addons
Let's say you have an Ember project for a social media site. Let's call it Pacebook.
Among the multiple pages it has, we are interested in the *feed page* and the *chat page*.

* We are gonna split Pacebook into multiple [engine projects](http://ember-engines.com/).

> You could split the root level routes into separate engines or so. Any portion of your project that seems like it can run separately as a standalone app qualifies to be migrated to an engine.

For this project we are gonna split *feed* and *chat* pages into separate engine projects.

* Now find common code among these engines and move them to addons. You could also club common CSS as well. Good examples of these would be models, common logic, common css etc.

Here are are gonna move some *common css(design system)* and the *notifications widget* into separate addons.

Now you have something like this:

```
Pacebook
├── host-app
├── design-system-addon
├── notifications-widget-addon
├── feed-page-engine
└── chat-page-engine
```
Where Host-app host is your original project and Engine-* are the ones you split.

> Now let's get down to business.

#### Adding yarn workspace

We will be using [yarn workspace](https://classic.yarnpkg.com/en/docs/workspaces/) to create our monorepo.

1) Move your file structure to this kinda layout (not necessary though)
```
Pacebook
├── host-app
├── addons
│   ├── design-system
│   └── notifications-widget
└── engines
    ├── feed-page
    └── chat-page
```

2) In your project's root create a `package.json` file and add these lines

###### package.json
```
{
  "private": true,
  "workspaces": ["engines/*", "addons/*"]
}
```

3) Include engine and addon version's in package.json of the respective apps.
one common trick is to keep engine and addon dependency as '*' in package.json to the host to keep them in the latest version always.

4) Remove all node_modules and yarn-lock or package-lock files from all your folders in the project.

5) run `yarn install` from the root folder of Pacebook.

[Here is a sample repo with yarn workspace](https://github.com/MalayaliRobz/ember-monorepo-demo)

> Congrats you have successfully migrated to yarn workspaces!

#### Adding Lerna to your workspace

[Lerna](https://lerna.js.org/) is a tool to help you to maintain your monorepo packages.
> Lerna is a tool that optimizes the workflow around managing multi-package repositories with git and npm.

1. install lerna
2. run `lerna init` to create lerna.json file
3. add these two lines to lerna.json to have it work with yarn workspaces
```
{  
  ...
  "npmClient": "yarn",
  "useWorkspaces": true,
}
```

Lerna makes it easy to automate stuff like:
 * updating package version of addons or engines
 * run commands in each workspace project in parallel
 * find when package had changed since last release

Full list of lerna commands can be found [here](https://github.com/lerna/lerna).

These might come in handy in your CI where you would wanna run test cases or publish for just the changed engine or addon.

#### Why not use in-repo addons? ([taken from here](https://medium.com/square-corner-blog/ember-and-yarn-workspaces-fca69dc5d44a))
Ember CLI’s original solution for simultaneously developing addons is in-repo addons, which have never required any npm link shenanigans. We have a few reasons for not using them:

* In-repo addons cannot have their own test suites. Test files go into the host application. We want our tests to be as decoupled as our application code.
* We may want to publish certain addons for use by other Ember applications. In-repo addons are not real NPM packages so they can’t be published on their own.
* Similarly, we want to allow teams to escape from the monorepo in the future if it makes sense for their product. Once their code is in a “real” addon with its own dependencies and test suite, they can move the files to another repository without much effort.
* Naming is hard—our “real” addons are inside the same git repository, so aren’t they “in-repo” addons? 🤯 To disambiguate the two patterns, we use the term “monorepo addon” for addons in our Yarn workspace

#### Ok now you have adopted monorepo for you project. Now what?

* Unity and move your eslint, prettier, template-list config to the root. That way you have only one config across modules.
* [Update ember try to take advantage to workspace](https://github.com/ember-cli/ember-try#workspaces). Believe me, your try test suit will run much faster now.
* While upgrading packages run `yarn upgrade-interactive` and `yarn outdated`. You don't have to update packages in multiple repos now. All the packages stay in sync.

### Wrapping up
While converting your project to a monorepo gives great advantages, it has some caveats as well such as settings up tooling to get it all working, not all tools may support this file structure, onboarding new member to the team etc...

Sometime you might not even need monorepo especially when something like [embroider](https://github.com/embroider-build/embroider) is on the rise.

You mostly don't need a monorepo when:
* your team size is small
* project source code is not that big
* Can't split code into modules or there is no need

At the end, it mostly up to developers working on a project and weighing up risks and benefits of monorepo for your project.








