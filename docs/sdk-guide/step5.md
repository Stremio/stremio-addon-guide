---
title: 5. Deploying
---


> **Note:** Although deploying is recommended, there is also the alternative of using [localtunnel](https://github.com/localtunnel/localtunnel) to host your add-ons locally.

Stremio add-ons require hosting in order to be published. You will need a NodeJS hosting solution, as Stremio Add-ons made with the Stremio Add-on SDK are NodeJS apps.

We recommend:

- [Now.sh](https://zeit.co/) - [free with some restrictions](https://zeit.co/pricing)
- [Heroku](https://www.heroku.com) - [free with some restrictions](https://www.heroku.com/pricing)
- [cloudno.de](https://cloudno.de) - [free for up to 150k requests/month](https://cloudno.de/pricing)
- [Evennode](https://www.evennode.com) - [free for 7 days trial](https://www.evennode.com/pricing)

We hugely recommend using `Now.sh`, as it is extremely easy to use.

You can also check this very comprehensive [guide by nodejs](https://github.com/nodejs/node-v0.x-archive/wiki/node-hosting).

Stremio add-ons are deployed just like regular nodejs apps, so follow the nodejs instructions provided by your particular service provider.

If you've built a great add-on and need help with hosting your add-on, you are welcome to contact us at [addons@stremio.com](addons@stremio.com)

### Publishing to Stremio

If you want your add-on to appear in the list of Community add-ons in Stremio, you can call the `publishToCentral` method at start up.

```js
const { addonBuilder, publishToCentral } = require("stremio-addon-sdk")

publishToCentral('https://example.com/manifest.json')
```

Of course, you need to substitute the example URL with the actual one where the add-on is hosted.

> It's recommended to test your add-on before you publish it. If your URL leads to а broken add-on, it will be silently accepted but it will not be listed in the third party add-ons catalog.
