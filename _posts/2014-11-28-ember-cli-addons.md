----
layout: post
title: Creating Ember CLI addons
----
{{ page.title }}
================
In **ember-cli** it is easy to reuse code by creating addons. Addons are Ember Components that are published and shared with [npm](https://www.npmjs.org/).


This post will take us through the process of creating an addon for generating MD5 hashes given some input.

By the end we will have a component that we can use like this

    {{md-5 value='my string'}} // 2ba81a47c5512d9e23c435c1f29373cb



## Creating an addon

To generate the structure of an addon use

    ember addon ember-cli-md5

Note that we use `addon` instead of `new`.

For this addon we need a library to generate MD5 hashes:

    bower install --save blueimp-md5

Then we import the library in the `brocfile.js` to make the application testable from the application. By running `ember server` the application can be tested locally without publishing or installing it locally in another project. More on this later.

    // brocfile.js
    app.import('bower_components/blueimp-md5/js/md5.min.js')
    module.exports = app.toTree();

`Index.js` in the root folder is the entry point of the addon. Here we have to import the same library file again, this time so that the addon can consume it.

    // index.js
    module.exports = {
      name: 'ember-cli-md5',
      included: function(app) {
        this._super.included(app);

        app.import('bower_components/blueimp-md5/js/md5.min.js');
      }
    };


Lets start using this library by making the component.

    ember g component md-5

From within this component we can add the logic we want

    // app/components/md-5.js
    export default Ember.Component.extend({
      value: null,
      md5 : function(){
        if(Ember.isEmpty(this.get('value'))){
          return "";
        }
        return md5(this.get('value'));
      }.property('value')
    });

And we need a place to show this hashed value

    // app/templates/components/md-5.hbs
    {{md5}}


The addon is almost complete. We just have to add the `blueprint` which tells the addon to download the javascript library. Up until now we have just referenced the downloaded version in `app.import(...)`, without specifying how the consumer of the addon should get the library.

    ember g blueprint md-5

In this blueprint we specify that we want to download the library from Bower using the `afterInstall` hook.

    // blueprints/md-5/index.js
    module.exports = {
      description: '',
      normalizeEntityName: function() {},
      afterInstall: function(options) {
        return this.addBowerPackageToProject('blueimp-md5');
      }
    };

This makes the installation a 2 step process. First installing the addon and then generate the blueprint which downloads the library.


## Check that it works
When developing an addon you might want to test to see if it works. The addon comes with a folder called `tests/dummy` which has an application where we can use our components. Since this is a small addon we will use the component in `application.hbs`

    // tests/dummy/app/templates/application.hbs
    <h2 id='title'>MD5</h2>
    <div>
      {{input type=text value=string}}
    </div>
    <div>
      {{md-5 value=string}}
    </div>

By running `ember server` we can now test that the component works by hashing input on the fly

If you want to test the addon in another project you can install it locally.

    // In the other project
    npm install ../ember-cli-md5
    ember g md-5

## Updating metadata
Each npm package has some metadata like version and URL for the repository. This can be changed in `package.json`.

`version` has to be incremented each time you do a `npm publish`.
`repository` is the link to where the source code is located.
`keywords` specifies `ember-addon` so **ember-cli** recognize it as an addon.

## Publishing
Now that we have seen that the component does what we want, it is time to share it with the world. If you have never published a npm package before, you'll need to create an account on npmjs.org. This can be done from the commandline:

    npm adduser

Now we are ready to do a publish

    npm publish
