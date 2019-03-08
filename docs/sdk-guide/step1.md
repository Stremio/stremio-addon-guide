---
title: 1. Get started
---

To initialize your first add-on, we will use the `addon-bootstrap` utility which is provided by `stremio-addon-sdk`.

The `addon-bootstrap` utility will ask you a few questions about your add-on name, description and what it will provide. Don't worry - you can always change this later!

## Deciding what your add-on will do

If you only intend to provide catalogs (content in Board/Discover), then you only need the `catalog` resource. Similarly, if you only want to provide streams, just select the `stream` resource.

**For more details, read [the Resources explanation](https://github.com/Stremio/stremio-addon-sdk/blob/master/docs/api/README.md) and [Content types](https://github.com/Stremio/stremio-addon-sdk/blob/master/docs/api/responses/content.types.md).**

## Initialize the add-on

```bash
npm install --global stremio-addon-sdk # requires "sudo" if you're on Linux
addon-bootstrap hello-world
```

Now, you will be asked various things about your add-on, such as name, description, resources it provides and types of content it will have.

After you've completed the steps, do:

```bash
cd hello-world
npm install
npm start -- --launch
```

**Congratulations!** You've created your first Stremio add-on. After the last step, the web version of Stremio will be opened in your browser with the add-on pre-installed!


## Structure

The initialized add-on will have the following structure:

* `addon.js`: this is the JS module that exports the [`addonInterface`](https://github.com/Stremio/stremio-addon-sdk/tree/master/docs#addongetinterface), which is a generic interface to your add-on; it will end in `module.exports = builder.getInterface()`
* `server.js`: this is the executable JS file that will start an HTTP server and optionally launch/install your add-on if you pass `--launch` or `--install`

This pattern is useful and encouraged because it allows your add-on to be used locally by only requiring `addon.js`
