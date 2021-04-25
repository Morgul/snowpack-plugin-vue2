# Snowpack Vue 2 Plugin

This is a [snowpack][] plugin for Vue v2.X, based off of the official Vue v3 plugin, [@snowpack/plugin-vue][plugin-vue]. The primary goal of this plugin is to provide an 'adapter' layer for [@vue/component-compiler-utils][compiler-utils] and [vue-template-compiler][] to reuse as much of the official snowpack vue plugin as possible. With that goal in mind, this repo is set up with `/plugin` corresponding to a copy of the `/plugins/plugin-vue` code, so that  should there ever be interest in making snowpack work with Vue2, the effort to port my changes to the vue plugin is minimal.

## Why Vue 2?

Currently, even https://vuejs.org/ points to the v2.x documentation. It's not that Vue 3 is bad, by any means. It will eventually, take over as the leading version of Vue. The problem is, _most_ libraries haven't updated yet, including a lot of the popular front-end frameworks. They won't have alpha versions for several more months, and probably won't have production versions till year's end. And those are just the popular, large projects. As a developer of dozens of Vue sites, I doubt I'll have my projects converted for _at least_ a year, if not two.

I don't know about you, but I don't see why I should have to wait to start using [Snowpack][snowpack].

## Usage

Simply install the plugin:

`npm`:
```bash
$ npm install -D @morgul/snowpack-plugin-vue2
```

`yarn`:
```bash
$ yarn add -D @morgul/snowpack-plugin-vue2
```

Then, add it to your `snowpack.config.js`:

```json5
// snowpack.config.json
{
  "plugins": [
    "@morgul/snowpack-plugin-vue2"
  ]
}
```

## Development

This plugin is definitely me scratching my own itch. And I haven't even decided if I _like_ [snowpack][] yet. But there's a need, and while there's about three or four vue2 plugins... none of them worked when I tried. So, here's a more formal engineered solution.

### Vue Library choice

Digging through the source code to [vue-loader][], I came upon [@vue/component-compiler-utils][compiler-utils], which appears to have been created after [@vue/compiler-sfc][compiler-sfc], with the same API, as a way of centralizing the code for doing all of this. I agree that something like [@vue/compiler-sfc][compiler-sfc] is required, but that's Vue 3 only, so I'm glad they backported that design to Vue2. It makes my life significantly easier.

_Note: As more development has happened, it's become clear that [@vue/component-compiler-utils][compiler-utils] doesn't do everything that [@vue/compiler-sfc][compiler-sfc], instead relying on [vue-loader][] to do some of it. That's fine, we can slowly intriduce [vue-loader][] code as needed to handle these edge cases._

### Project Guidelines

These are the guidelines for working on this project.

* **Absolutely no** modifications to [@vue/component-compiler-utils][compiler-utils] or [vue-template-compiler][].
* **Minimal to no** modifications to [@snowpack/plugin-vue][plugin-vue].
* All adaption code **must** live in `/compiler`.
* All plugin code **must** live in `/plugin`.
* **Where possible**, external packages are used without modification, only adaptation.

### Project layout

The bulk of this project's code lives in `/compiler`. The `/plugin` is a verbatim copy of [@snowpack/plugin-vue][plugin-vue], with as few changes as possible. (Even the original `package.json` is kept around.) There is also a sample project contained in `/example`, to act as both a test for the plugin, and a usage guide.

[snowpack]: https://www.snowpack.dev/
[plugin-vue]: https://github.com/snowpackjs/snowpack/tree/main/plugins/plugin-vue
[vue-loader]: https://github.com/vuejs/vue-loader
[compiler-utils]: https://github.com/vuejs/component-compiler-utils
[compiler-sfc]: https://github.com/vuejs/vue-next/blob/master/packages/compiler-sfc/
[vue-template-compiler]: https://github.com/vuejs/vue/tree/dev/packages/vue-template-compiler
