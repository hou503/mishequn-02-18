
---
title: Webpack5
date: 2020-6-1 20:00:00
tags:  Webpack
category: Webpack
description: Webpack5 is expected to be released in early 2020 and has been in the limelight since the alpha version, but this update focuses on long term caching, tree shakking and es6 packaging.
---
Webpack5 is expected to be released in early 2020 and has been in the limelight since the alpha version, but this update focuses on long term caching, tree shakking and es6 packaging.

Webpack is the hottest module packaging tool in modern front-end development, only through a simple configuration, you can complete the module loading and packaging. How does it do that by configuring some plug-ins, you can easily achieve the construction of the code?

This article won't discuss the content to be updated in webpack5, I believe most front-end students will just have a simple configuration for webpack, and there are good packages for webpack like vue-cli, umi, etc., but it's not good for us. Especially when you want to do some personalized customization for business scenarios. In this article, we will take you step by step to build the ultimate front-end development environment from the existing major version of webpack, webpack4.

# Several ways to install webpack

- global: run the `webpack index.js`
- local: run `npx webpack index.js`

Avoid global installation of webpack (scenario where multiple projects are packaged with different versions of webpack), use `npx`.

# Entry

## Single entry
```js
// webpack.config.js

const config = {
  entry: {
    main: "./src/index.js"
  }
};
```

## Multiple entry
// webpack.config.js

const config = {
  entry: {
    main: "./src/index.js",
    sub: "./src/sub.js"
  }
};

# Output

## Default configuration
```js
// webpack.config.js
const path = require('path');
...

const config = {
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
};

module.exports = config;
```

## Multiple entry
If the configuration creates multiple individual "chunks" (e.g., using multiple entry points or using a plugin like CommonsChunkPlugin), placeholders (substitutions) should be used to ensure that each file has a unique name.

```js
// webpack.config.js
const path = require('path');
{
  entry: {
    main: './src/index.js',
    sub: './src/sub.js'
  },
  output: {
    filename: '[name].js',
    path: path.resolve(__dirname, 'dist')
  }
}

// Write to hard disks：./dist/main.js, ./dist/sub.js
```




