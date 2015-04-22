# Hacks at integrating Browserify & Meteor
Attempts and thoughts at getting Javascript libraries written with `require` syntax into a Meteor build flow

## Javascript
Have a single file that `require`s the npm modules you need, exposing them as globals (because Meteor loves globals)

```js
React = require("react");
mui = require('material-ui');
```

Then browserify that file into somewhere in your Meteor app. If you put it not in client/ or server/ then boom, isomorphic Javascript, aka yes, you can use it on both sides.

```sh
browserify main.js > app/bundle.js
```


## Less
The less compiler will go and find files wherever they are, so you just need to `@import` carefully. Using the [`grove:less`](https://github.com/grovelabs/meteor-less) package, you can do this in your `main.less` file and put it amongst your other imports.

```less
@material-ui: "../node_modules/material-ui/src/less";
@import "@{material-ui}/scaffolding.less";
@import "myVariables.less";	// override defaults
@import "@{material-ui}/components.less";
```

## Sass
Pretty sure this would work the same as Less

## Downsides
* You're deliberately muddying your scope, but that's the Meteor way right now.

## Possible Improvements
* Automating the `browserify` bundling command, which you could do with a file watcher on your global-exposing main.js file. Another non-Meteor tool to run though
* Putting the bundle js in `packages/foo` with a proper `package.js` file exposing the globals, and add it as a Meteor package to make sure it gets loaded in before your application code