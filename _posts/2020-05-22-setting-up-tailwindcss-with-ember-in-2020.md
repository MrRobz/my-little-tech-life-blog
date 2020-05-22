---
layout: post
current: current
class: post-template
subclass: post tag-ember-addon
navigation: true
title: Setting up TailwindCSS with ember with in-build purgeCSS
cover: /assets/uploads/tailwind-ember.png
tags: ember tailwind
author: robin
---
Well, integrating tailwind with ember has become easier than ever. TLDR; as of Tailwind CSS v1.4  tailwind supports built-in PurgeCSS.

### Adding tailwindcss to an ember app

##### Let's start by creating a new ember app:

```
ember new your-project --yarn
cd your-project
```

##### Install PostCSS

```
yarn add ember-cli-postcss tailwindcss postcss-import -D
```

##### Generate Tailwind configuration file

```
mkdir app/tailwind
npx tailwind init app/tailwind/config.js --full
```

Note: you probably don't want to add --full in a real project.

Add purge config to the top of tailwind config file:

```javascript
const EmberApp = require('ember-cli/lib/broccoli/ember-app');
const isProduction = EmberApp.env() === 'production';

module.exports = {
  purge: {
    enabled: isProduction,
    content: [
    './app/index.html',
    './app/templates/**/*.hbs',
    './app/components/**/*.hbs'
    ]
  },
....
```

These settings will make sure that you don't ship any unwanted tailwind CSS to production build. More info [](https://tailwindcss.com/docs/controlling-file-size/)[here](https://tailwindcss.com/docs/controlling-file-size/)

##### Update build pipeline to include plugins

```javascript
// ember-cli-build.js
'use strict';

const EmberApp = require('ember-cli/lib/broccoli/ember-app');

module.exports = function(defaults) {
  let app = new EmberApp(defaults, {
    postcssOptions: {
      compile: {
        plugins: [
          {
            module: require('postcss-import'),
            options: {
              path: ['node_modules']
            }
          },
          require('tailwindcss')('./app/tailwind/config.js')
        ]
      }
    }
  });
  return app.toTree();
};
```

##### Create new CSS files and import Tailwind

Create `app/styles/components.css` and `app/styles/utilities.css` then update `app.css`

```css
@import "tailwindcss/base";

@import "tailwindcss/components";
@import "components.css";

@import "tailwindcss/utilities";
@import "utilities.css";
```

Now you can start to add Tailwind classes to your project, add additional configuration including custom components and utilities.

Reference for this was taken from https://github.com/chrism/emberjs-tailwind-purgecss