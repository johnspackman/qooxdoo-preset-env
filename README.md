# qooxdoo-preset-env 

> A Babel preset that compiles [ES2015+](https://github.com/tc39/proposals/blob/master/finished-proposals.md) down to ES5 by automatically determining the Babel plugins and polyfills you need based on your targeted browser or runtime environments.

**::NOTE::** This is probably **not** what you want - read the description below before continuing.

The https://github.com/babel/babel-preset-env groups a set of babel plugins into a preset that will implement ES6->ES5 transpiling; this works great except that a recent update to one of the plugins causes a massive slow down (eg in the region of x100).  Because it is not possible to override the modules that an npm module depends on (or at least, I've not been able to find a way that works), the easiest solution is to copy babel-preset-env and change the version numbers.  This may be fixed in Babel 7.x, but at the moment qxcompiler uses 6.x so this plugin is necessary.

This repo is a copy of [v1.6.1 of babel-preset-env](https://github.com/babel/babel-preset-env/tree/v1.6.1).

The problem is that the babel-preset-env uses a later version of babel-plugin-transform-es2015-block-scoping which has a bug which causes a massive slow down of parsing because it uses an array scan instead of a simple lookup. The @qooxdoo/preset-env locks babel-plugin-transform-es2015-block-scoping at 6.15.0 which does not have the problem (this the solution found at https://github.com/babel/babel/issues/4795).

It is necessary to maintain a separate package because it is not possible to get npm to use a specific version in a nested package; yarn does support such a thing, but only if the nested package has a dependency which allows it, and in this case yarn insists on using the newer, broken module.  I also found a solution with npm but it does not work consistently, and ultimately it too will revert to using the newer version.

```sh
npm install @qooxdoo/preset-env --save-dev
```

Without any configuration options, babel-preset-env behaves exactly the same as babel-preset-latest (or babel-preset-es2015, babel-preset-es2016, and babel-preset-es2017 together).

```json
{
  "presets": ["@qooxdoo/preset-env"].map(require.resolve)
}
```

