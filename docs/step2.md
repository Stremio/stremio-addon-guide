---
id: step2
title: Step 2 - Testing the add-on
---

Previously, we covered the basic structure of the add-on manifest. Now it is time to make sure it is working properly. Let's test it!

## Serving

> **Note for serving Stremio add-ons**
>
> Every add-on must provide **CORS** headers for its resources. Stremio **cannot** make use of an add-on that does not support CORS.

For easy serving we'll use the `http-server` node module. Of course, you can use your preferred HTTP server. Just don't forget to configure it to serve CORS headers.

```sh
npm -g install http-server
http-server --cors -c-1
```

By default `http-server` serves content on port 8080. It should be printed in your console window, the moment you run it.

The `-c-1` argument disables the HTTP cache headers. This will be helpful during the add-on development. The cache is useful in production where your add-on will be stressed by a large number of requests.

You can leave this server running during the next steps, so that your changes are available immediately.

Your add-on's manifest is located at `http://127.0.0.1:8080/manifest.json`.

## Install the add-on

Now let's install your new add-on. Go to Stremio. Open the add-ons configuration either by clicking on the puzzle icon in the upper right corner or by opening it from `Settings`. Now you can copy the add-on URL from here and paste it in the field labeled `Add-on Repository URL`. You should be prompted with the installation dialog. Just press `Install` and your add-on will appear in the `My add-ons` section.

![Installed add-on](/img/install.png)

You can also see the request for your manifest in the `http-server`'s output. In fact, this is where every request to from Stremio to the add-on will be logged. No matter what kind of server or framework you use, it's advised to check your logs regularly. This will help you easily identify possible errors.

You successfully installed your first add-on. It is useless in it's current form so we recommend you remove it by clicking on the `Uninstall` button. You will install it again after you add functionality.

> Note that reinstalling the add-on by clicking `Uninstall` followed by clicking `Install` does **not** reload the manifest, therefore your changes **will not be applied**. You must install it by typing/pasting the URL to your add-on's manifest in the box labeled "Add-on Repository URL".

Summary
---

Testing your add-on is a very important step of the development process. We recommend you test it regularly so you don't have to search for an error after many changes.

In the next step we will begin adding functionality, namely the catalog.
