# Intro

## What is Stremio

Stremio is a modern media center that's a one-stop solution for your video entertainment. It features an add-on system that is responsible for all of the available content.

The purpose of this guide is to teach you how Stremio add-ons work and how to build one yourself, using our Add-on SDK. This will allow you to extend Stremio with your favorite content!

## Motivation

There are plenty of reasons to create your own add-on, including but not limited to:

* You want your favorite content in Stremio
* You want a fun weekend project to enhance your coding skills with
* You are running a video entertainment service and want to promote it in Stremio
* You are participating in Stremio Add-on competition

## Requirements

To build an add-on using our SDK, you only need:

* Basic programming knowledge (JavaScript)
* A recent version of NodeJS (which [you can download from their website](https://nodejs.org/en/download/))


## Get started

@TODO quick explanations about types of addons

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


## Video tutorials and other guides

There are also video tutorials on our [YouTube channel](https://www.youtube.com/channel/UC_OgO2Ag911Y49ZeArzy1_w).

If you want to learn how to build an add-on in any programming language, check out our [universal guide](/intro).

There's also quick examples (with step-by-step tutorials) for Python, Ruby, NodeJS Express and others [here](https://github.com/Stremio/stremio-addon-sdk/blob/master/docs/examples/README.md).


