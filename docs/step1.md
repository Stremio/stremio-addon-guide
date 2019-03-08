---
title: 1. The add-on manifest
---

This is the first step in our add-on guide. We'll create the add-on manifest for a very basic add-on.

The add-on manifest should be served on `/manifest.json` route in your add-on.

Its mandatory fields are as follows:

```json
{
    "id": "my.first.stremio.add-on",
    "version": "1.0.0",
    "name": "Hello, World",
    "description" : "My first Stremio add-on",
    "logo": "https://www.stremio.com/website/stremio-logo-small.png",
    "resources": [],
    "types": []
}
```

This is the skeleton of a Stremio add-on's manifest. The `logo` field is optional, but your add-on will look much nicer and more recognizable if it has a logo so we strongly encourage you to add one.

Let's take a closer look at the manifest.

The `id` field is an arbitrary string that distinguishes your add-on from others. It's a good practice to use dot-separated identifier like `com.stremio.filmon`.

The `version` must be a valid [Semantic Versioning](https://semver.org/) string describing your add-on's version.

The next fields - `name` and `description` - are descriptive. They explain to the user what is the purpose of your add-on and what content can they expect from it.

The `logo` is displayed next to your add-on name and description. It should be a URL to a square image with resolution of 256 x 256px.

The `resources` and `types` are arrays of features that your add-on supports. We will take a closer look into this latter on.

> You can also take a look at the [complete manifest reference](https://github.com/Stremio/stremio-addon-sdk/blob/master/docs/api/responses/manifest.md).

Bootstrap the add-on
---

To see how this works, let's create `manifest.json` with the contents above and serve it via HTTP. You can successfully install it in Stremio.

First we will create a new directory where our add-on will be located. Then we will populate this directory step-by step.

<!--DOCUSAURUS_CODE_TABS-->
<!--bash-->
```bash
mkdir my-stremio-addon
cd my-stremio-addon
cat > manifest.json
{
    "id": "my.first.stremio.add-on",
    "version": "1.0.0",
    "name": "Hello, World",
    "description" : "My first Stremio add-on",
    "logo": "https://www.stremio.com/website/stremio-logo-small.png",
    "resources": [],
    "types": []
}
^D
```
<!--cmd-->
```batch
mkdir my-stremio-addon
cd my-stremio-addon
copy con manifest.json
{
    "id": "my.first.stremio.add-on",
    "version": "1.0.0",
    "name": "Hello, World",
    "description" : "My first Stremio add-on",
    "logo": "https://www.stremio.com/website/stremio-logo-small.png",
    "resources": [],
    "types": []
}
^Z
```
<!--END_DOCUSAURUS_CODE_TABS-->

The `^D`/`^Z` in the last line means you need to press the <kbd>Control</kbd> key together with the corresponding letter on the keyboard. This signals to the shell that we are finished typing our file.

This is it. We now have the minimal required manifest to install our add-on.

Summary
---

Now you have an idea about the skeleton of the manifest - the most important part of a Stremio add-on. Go ahead to the next step where we will test it.

