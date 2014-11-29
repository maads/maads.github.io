---
layout: post
title: Deploying ember-cli apps and addons to Github Pages
---

{{ page.title }}
================
Given that you are hosting the code for your ember-cli application or addon on Github, you might want to publish it to [Github Pages](https://pages.github.com/). This is especially nice if you are creating an addon and want to show a demo of how it works.

## Making it ready
When hosting on Github Pages the url for your project site will be

    http://yourusername.github.io/projectname

To tell the ember-cli application that we are not running on the root domain, we have to update the `baseURL` in `environment.js`

    // apps: config/environment.js
    // addons: tests/dummy/config/environment.js
    if (environment === 'production') {
      ENV.baseURL = '/projectname'
    }

Without changing this, the app or addon will look for the `assets` folder in `username.github.io/assets` when it will be located in `username.github.io/projectname/assets`


## Publishing
Now that we have changed the `baseURL` we are ready to build the application and publish it. Github Pages serves the site in its own branch called `gh-pages`. To build, create the new branch and publish the site we can use the following script

    // publishToGithubPages.sh
    #!/bin/bash
    git branch -D gh-pages
    git push origin --delete gh-pages
    git checkout -b gh-pages
    ember build --environment production
    git rm -rf app addon config tests
    git rm -rf Brocfile.js bower.json package.json testem.json
    git rm -rf .bowerrc .editorconfig .jshintrc .travis.yml
    mv dist/* .
    rm -rf dist
    git add .
    git commit -m "Publishing to github pages"
    git push origin gh-pages
    git checkout master
